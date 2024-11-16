---
author: Jesse Morgan
categories:
- Uncategorized
date: "2015-05-20T14:27:30Z"
guid: http://morgajel.net/?p=1681
id: 1681
title: The Pain and Fury of vmware-cli on CentOS 7, Part 2
url: /2015/05/20/1681
---

[Last we left off](http://morgajel.net/2015/05/20/1676) with a functioning vmware-cli and no way to replicate this in ansible. Lets fix that. We’ll be using FPM to create an RPM containing all of the files that were created by the installer (including cpan modules).  
But first, we need to figure out which files need to be packaged. VMWare aaallmost kinda makes this easy for us- each file it installs is tracked in /etc/vmware-vcli/locations (excluding cpan files). Parsing through this, we see

- a list of all the CPAN modules installed
- a list of directories it’s created
- a list of the files it’s created (including symlinks

## Step 1: Identifying vmware-cli Files to Package

We’ll hold off on CPAN modules for the moment and jump ahead to directories and files. We can trim this list down quite a bit because we can ignore the filenames in directories that the installer created- in other words, we can include /foo/bar/ and it’ll automatically include /foo/bar/baz.pm. This allows us to cut down our 1400+ files and directories down to about 63- most of which are symlinks and can be simplified with a wild card. Once we refactor all that info, the end result is something like this:

```
/etc/vmware-vcli/locations /usr/share/perl5/VMware/ /usr/share/perl5/WSMan/ /opt/vmwarecli/ /usr/bin/dcli /usr/bin/esxcfg-* /usr/bin/esxcli /usr/bin/resxtop /usr/bin/svmotion /usr/bin/vicfg-* /usr/bin/vifs /usr/bin/vihostupdate /usr/bin/vihostupdate35 /usr/bin/viperl-support /usr/bin/vmkfstools /usr/bin/vmware-cmd /usr/bin/vmware-uninstall-vSphere-CLI.pl /etc/vmware-vcli/config
```

Save this list for later.

## An Aside- Thanks for the mess, CPAN.

If you recall, we took a snapshot of our perl modules before and after our installation. You may remember us doing the following (don’t do this right now!!!):

```
perl -e 'use doesntexist;'
Can't locate doesntexist.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at -e line 1.
BEGIN failed--compilation aborted at -e line 
find /usr/local/lib64/perl5 /usr/local
```

then later:

```
find /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 >/tmp/final.list
```

Unfortunately that’s only part of the story. When we rerun that first command:

```
perl -e 'use doesntexist;'
Can't locate doesntexist.pm in @INC (@INC contains: /root/perl5/lib/perl5/x86_64-linux-thread-multi /root/perl5/lib/perl5 /root/perl5/lib/perl5/x86_64-linux-thread-multi /root/perl5/lib/perl5 /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at -e line 1.
BEGIN failed--compilation aborted at -e line 1.
```

What the heck? where did all those /root/perl5/ paths come from? from our /root/.bashrc:

```
<...>
export PERL_LOCAL_LIB_ROOT="$PERL_LOCAL_LIB_ROOT:/root/perl5";
export PERL_MB_OPT="--install_base /root/perl5";
export PERL_MM_OPT="INSTALL_BASE=/root/perl5";
export PERL5LIB="/root/perl5/lib/perl5:$PERL5LIB";
export PATH="/root/perl5/bin:$PATH";

export PERL_LOCAL_LIB_ROOT="$PERL_LOCAL_LIB_ROOT:/root/perl5";
export PERL_MB_OPT="--install_base /root/perl5";
export PERL_MM_OPT="INSTALL_BASE=/root/perl5";
export PERL5LIB="/root/perl5/lib/perl5:$PERL5LIB";
export PATH="/root/perl5/bin:$PATH";
```

These… these were not here before. So what is in /root/perl5/? The big takeaways are:

```
/root/perl5/lib/perl5/CPAN/Meta/*
/root/perl5/lib/perl5/ExtUtils/*
/root/perl5/lib/perl5/JSON/*
/root/perl5/lib/perl5/Parse/*
/root/perl5/bin/instmodsh
```

\*sigh\* I have no idea when or why or how these were inserted. I don’t know if that was CPAN not cleaning up after itself or what. all I know is that a script running as a user might now use different libraries than one running as root. I don’t know if those libs are important, but judging from the contents I’m presuming they’re only for building packages and are not functionally involved (I hope).

So we’ll blissfully ignore them and hope it doesn’t bite us later on.

## Step 2: Identifying CPAN-Installed Files to Package

ok, so we have our two files- /tmp/pristine.list and /tmp/final.list. We can use the following to gather a list of new files:

```
cat /tmp/pristine.list /tmp/final.list |sort|uniq -u >/tmp/unique.list
```

We can do roughly the same thing with this list as we did with /etc/vmware-vcli/locations- ignore the files under any directories we’ve created. In addition, we can ignore any directories we’ve already documented, like /usr/share/perl5/VMware and /usr/share/perl5/WSMan/. Unfortunately, there are some troublesome directories- /usr/local/share/perl5 and /usr/local/lib64/perl5 are pretty generic- if we include those, it may cause conflicts with other RPMs down the road- I honestly don’t know if this is the case, so we’ll hope for the best. We end up with the following list:

```
/usr/lib64/perl5/auto/Locale
/usr/lib64/perl5/auto/Params
/usr/lib64/perl5/perllocal.pod
/usr/local/lib64/perl5
/usr/local/share/perl5
/usr/share/perl5/Locale/Maketext
/usr/share/perl5/Params
```

Save this list for later.

## Final Packages

Time to bundle everything up… But we’ll need fpm installed first:

```
yum install rpm-build ruby-devel gcc gem
gem install fpm
```

From here, we can take those file/directory lists and feed them a single file:

```
ls -d /etc/vmware-vcli/locations /usr/share/perl5/VMware/ /usr/share/perl5/WSMan/ /opt/vmwarecli/ /usr/bin/dcli /usr/bin/esxcfg-* /usr/bin/esxcli /usr/bin/resxtop /usr/bin/svmotion /usr/bin/vicfg-* /usr/bin/vifs /usr/bin/vihostupdate /usr/bin/vihostupdate35 /usr/bin/viperl-support /usr/bin/vmkfstools /usr/bin/vmware-cmd /usr/bin/vmware-uninstall-vSphere-CLI.pl /etc/vmware-vcli/config /usr/lib64/perl5/auto/Locale /usr/lib64/perl5/auto/Params /usr/lib64/perl5/perllocal.pod /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/share/perl5/Locale/Maketext /usr/share/perl5/Params > /tmp/fpm_final_list
```

now we’ll feed /tmp/fpm\_final\_list into FPM to create a shiny-new RPM:

```
fpm -s dir -d perl-Data-Dumper -a noarch --rpm-user root --rpm-group root -t rpm -n vmware-cli -v '6.0.0_2503617' --iteration 1 -C /  /etc/vmware-vcli/locations /usr/share/perl5/VMware/ /usr/share/perl5/WSMan/ /opt/vmwarecli/ /usr/bin/dcli /usr/bin/esxcfg-* /usr/bin/esxcli /usr/bin/resxtop /usr/bin/svmotion /usr/bin/vicfg-* /usr/bin/vifs /usr/bin/vihostupdate /usr/bin/vihostupdate35 /usr/bin/viperl-support /usr/bin/vmkfstools /usr/bin/vmware-cmd /usr/bin/vmware-uninstall-vSphere-CLI.pl /etc/vmware-vcli/config /usr/lib64/perl5/auto/Locale /usr/lib64/perl5/auto/Params /usr/lib64/perl5/perllocal.pod /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/share/perl5/Locale/Maketext /usr/share/perl5/Params
```

And because nothing is easy, this throws errors; apparently the current version of FPM doesn’t like symlinks (all of the files in /usr/bin). If we cut those out:

```
fpm -s dir -d perl-Data-Dumper -a noarch --rpm-user root --rpm-group root -t rpm -n vmware-cli -v '6.0.0_2503617' --iteration=1 -C / /etc/vmware-vcli /opt/vmwarecli /usr/lib64/perl5/auto/Locale /usr/lib64/perl5/auto/Params /usr/lib64/perl5/perllocal.pod /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/share/perl5/Locale/Maketext /usr/share/perl5/Params /usr/share/perl5/VMware /usr/share/perl5/WSMan
```

This builds fine, BUT we have no symlinks. Ugh this is kludgy, but we can make a fugly little postinstall script and have FPM include that (I hope)

```
echo '#!/bin/bash'> /tmp/vmlinker.sh ; echo 'cd /opt/vmwarecli/bin/ ; for i in * ; do ln -s /opt/vmwarecli/bin/$i /usr/bin/$i ; done' >>/tmp/vmlinker.sh
chmod 755 /tmp/vmlinker.sh
```

and now we can include it in our fpm command:

```
fpm -f --post-install /tmp/vmlinker.sh -s dir -d perl-Data-Dumper -a noarch --rpm-user root --rpm-group root -t rpm -n vmware-cli -v '6.0.0_2503617' --iteration=1 -C / /etc/vmware-vcli /opt/vmwarecli /usr/lib64/perl5/auto/Locale /usr/lib64/perl5/auto/Params /usr/lib64/perl5/perllocal.pod /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/share/perl5/Locale/Maketext /usr/share/perl5/Params /usr/share/perl5/VMware /usr/share/perl5/WSMan
no value for epoch is set, defaulting to nil {:level=>:warn}
Force flag given. Overwriting package at vmware-cli-6.0.0_2503617-1.noarch.rpm {:level=>:warn}
no value for epoch is set, defaulting to nil {:level=>:warn}
Created package {:path=>"vmware-cli-6.0.0_2503617-1.noarch.rpm"}
```

And there we go. Time to test it.

Before we go, we need to snapshot our machine yet again as “package-created.” We also need to copy our wonderful RPM down to our local machine so we can revert back to the raw snapshot and test it.

## Testing our RPM

Revert back to our Raw Snapshot, then copy our rpm to /tmp on it.

```
yum install /tmp/vmware-cli-6.0.0_2503617-1.noarch.rpm
```

Note that this will install perl-Data-Dumper as a dependency; why? because somehow the rpm was installed and I didn’t notice that it was still there and I don’t want to backtrack to have cpan build it. So now it’s a dependency- we’ll circle back to this in a moment- for now… does it work?

```
/opt/vmwarecli/bin/vmware-cmd --server vcenter.domain.com -l -h esxhost1 --username vcenteruser@domain.com --password 'somethingsecure'
/vmfs/volumes/54eaad0a-923ab37c-90b6-b82a72d4d796/hostname1/hostname1.vmx
/vmfs/volumes/54eaad24-7c233c62-970e-b82a72d4d796/hostname2/hostname2.vmx
<...>
```

SUCCESS!

Now that we know our RPM works, we can touch back on some more uncomfortable questions. Right now we have about 28 perl modules installed via CPAN (possibly more). Many of our other apps (such as morgnagplug and manubulon) are dependent on the RPM versions of some of those modules. When we install them, we could have conflicts down the road. So how many of those modules could be installed as RPMs?

```
module Devel::StackTrace
module Class::Data::Inheritable
module Convert::ASN1
module Crypt::OpenSSL::RSA
module Crypt::X509
module Exception::Class
module UUID::Random
module Archive::Zip
module Compress::Zlib
module Compress::Raw::Zlib
module Path::Class
module Try::Tiny
module Crypt::SSLeay
module IO::Compress::Base
module IO::Compress::Zlib::Constants
module HTML::Parser
module UUID
module Data::Dump
module SOAP::Lite
module URI
module XML::SAX
module XML::NamespaceSupport
module XML::LibXML::Common
module XML::LibXML
module LWP
module LWP::Protocol::https
module IO::Socket::INET6
module Net::INET6Glue
```

I’m not sure. Let’s find out.

Lets snapshot this as “vmware rpm installed”. As an aside, the snapshot functionality almost makes up for the grief vmware is putting me through.

## Which CPAN modules can be swapped for RPMs?

Here’s my plan of attack; for each module listed above,

1. **yum search modulename**; Not all modules will have RPMs, but this is the best way to find out if it’s possible to replace.
2. run **rpm -ql vmware-cli |grep modulename** ; Note that you should replace the :: with a slash, so Devel::StackTrace becomes Devel/StackTrace
3. delete matching files from the filesystem (we can always reinstall the RPM if we get in trouble, or even revert back to the “vmware rpm installed” snapshot).
4. **yum install moduleRPM** ; this should install ONLY one module, no dependencies; if it has dependencies, put it to the side.

I’m now going to run through each module and document it as I go

- Devel::StackTrace RPM exists, has no dependencies. Files removed, rpm installed, **vmware-cmd still works**
- Class::Data::Inheritable RPM exists, has no dependencies, Files removed, rpm installed, **vmware-cmd still works**
- Convert::ASN1 RPM exists, has no dependencies, Files removed, rpm installed, **vmware-cmd still works**
- Crypt::OpenSSL::RSA RPM exists, HAS DEPENDENCIES, **shelved**
- Crypt::X509 **No RPM currently exists**
- Exception::Class RPM exists, has no dependencies, Files removed, rpm installed, **vmware-cmd still works**
- UUID::Random **No RPM currently exists**
- Archive::Zip RPM exists, HAS DEPENDENCIES, **shelved**
- Compress::Zlib RPM exists, HAS DEPENDENCIES, **shelved**
- Compress::Raw::Zlib RPM exists, has no dependencies, Files removed, rpm installed, **vmware-cmd still works**
- Path::Class **No RPM currently exists**
- Try::Tiny RPM exists (perl-try-tiny), has no dependencies, Files removed, rpm installed, **vmware-cmd still works**
- Crypt::SSLeay RPM exists, has no dependencies, Files removed, rpm installed, **vmware-cmd still works**
- IO::Compress::Base **No RPM currently exists (probably installed with perl-IO-Compress, but it has dependencies)**
- IO::Compress::Zlib::Constants **No RPM currently exists (possibly installed with perl-IO-Compress, but it has dependencies)**
- HTML::Parser RPM exists, HAS DEPENDENCIES, **shelved (here be dragons)**
- UUID Difficult to Discern- UUID RPMs doesn’t seem to line up**; shelved.**
- Data::Dump **No RPM currently exists (not to be confused with Data::Dumper)**
- SOAP::Lite **No RPM currently exists**
- URI Difficult to discern… RPM exists, HAS DEPENDENCIES, **shelved**
- XML::SAX RPM exists, HAS DEPENDENCIES, **shelved**
- XML::NamespaceSupport RPM exists, has no dependencies. Files removed, rpm installed, **vmware-cmd still works**
- XML::LibXML::Common **No RPM currently exists (may be included in a different RPM)**
- XML::LibXML RPM exists, HAS DEPENDENCIES, **shelved**
- LWP RPM exists, HAS DEPENDENCIES, **shelved (here be dragons)**
- LWP::Protocol::https RPM exists, HAS DEPENDENCIES, **shelved (here be dragons)**
- IO::Socket::INET6 RPM exists, HAS DEPENDENCIES, **shelved**
- Net::INET6Glue **No RPM currently exists (may be included in a different RPM)**

Circling back, lets see if any of the libs with dependencies have had their dependencies installed

- Archive::Zip RPM exists, has no dependencies. Files removed, rpm installed, **vmware-cmd still works**

The rest are dead ends. We can now hypothesize that the following libs are acceptable in RPM form:

perl-Devel-StackTrace perl-Class-Data-Inheritable perl-Convert-ASN1 perl-Exception-Class perl-Compress-Raw-Zlib perl-Try-Tiny perl-Crypt-SSLeay perl-XML-NamespaceSupport perl-Archive-Zip

if we exclude the following paths from our FPM build process:

/usr/local/lib64/perl5/auto/Devel/StackTrace/ /usr/local/share/perl5/Devel/StackTrace.pm /usr/local/share/perl5/Devel/StackTrace/ /usr/local/lib64/perl5/auto/Class/Data/ /usr/local/share/perl5/Class/Data /usr/local/lib64/perl5/auto/Convert/ /usr/local/share/perl5/Convert/ /usr/local/lib64/perl5/auto/Exception/ /usr/local/share/perl5/Exception/ /usr/local/lib64/perl5/Compress/ /usr/local/lib64/perl5/auto/Compress/ /usr/local/lib64/perl5/auto/Compress/ /usr/local/lib64/perl5/auto/Try/ /usr/local/share/perl5/Try/ /usr/local/lib64/perl5/Crypt/SSLeay/ /usr/local/lib64/perl5/Crypt/SSLeay.pm /usr/local/lib64/perl5/auto/Crypt/SSLeay/ /usr/local/lib64/perl5/auto/XML/NamespaceSupport/ /usr/local/share/perl5/XML/NamespaceSupport.pm /usr/local/lib64/perl5/auto/Archive/ /usr/local/share/perl5/Archive/

Lets switch over to “package created” Snapshot and repackage our RPM with new dependencies and exclusions:

```
fpm -f --post-install /tmp/vmlinker.sh -s dir -d perl-Data-Dumper -a noarch --rpm-user root --rpm-group root -t rpm -n vmware-cli -v '6.0.0_2503617' --iteration=1 -C / \
 \
-d perl-Devel-StackTrace -d perl-Class-Data-Inheritable -d perl-Convert-ASN1 -d perl-Exception-Class -d perl-Compress-Raw-Zlib -d perl-Try-Tiny -d perl-Crypt-SSLeay -d perl-XML-NamespaceSupport -d perl-Archive-Zip \
 \
-x "**usr/local/lib64/perl5/auto/Devel/StackTrace/**" -x "**usr/local/share/perl5/Devel/StackTrace.pm" -x "**usr/local/share/perl5/Devel/StackTrace/**" -x "**usr/local/lib64/perl5/auto/Class/Data/**" -x "**usr/local/share/perl5/Class/Data**" -x "**usr/local/lib64/perl5/auto/Convert/**" -x "**usr/local/share/perl5/Convert/**" -x "**usr/local/lib64/perl5/auto/Exception/**" -x "**usr/local/share/perl5/Exception/**" -x "**usr/local/lib64/perl5/Compress/**" -x "**usr/local/lib64/perl5/auto/Compress/**" -x "**usr/local/lib64/perl5/auto/Compress/**" -x "**usr/local/lib64/perl5/auto/Try/**" -x "**usr/local/share/perl5/Try/**" -x "**usr/local/lib64/perl5/Crypt/SSLeay/**" -x "**usr/local/lib64/perl5/Crypt/SSLeay.pm" -x "**usr/local/lib64/perl5/auto/Crypt/SSLeay/**" -x "**usr/local/lib64/perl5/auto/XML/NamespaceSupport/**" -x "**usr/local/share/perl5/XML/NamespaceSupport.pm" -x "**usr/local/lib64/perl5/auto/Archive/**" -x "**usr/local/share/perl5/Archive/**" \
 \
/etc/vmware-vcli /opt/vmwarecli /usr/lib64/perl5/auto/Locale /usr/lib64/perl5/auto/Params /usr/lib64/perl5/perllocal.pod /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/share/perl5/Locale/Maketext /usr/share/perl5/Params /usr/share/perl5/VMware /usr/share/perl5/WSMan
```

Note: fpm’s excludes are picky as hell- you need doublequotes around each one as well as double wildcards in place of a leading / and trailing it if a directory.

Even then, this STILL leaves the directories, just not the contents of the directory.

I’m trying too hard. Let’s try something slightly different:

```
fpm -f --post-install /tmp/vmlinker.sh -s dir -d perl-Data-Dumper -a noarch --rpm-user root --rpm-group root -t rpm -n vmware-cli -v '6.0.0_2503617' --iteration=1 -C / \
 \
-d perl-Devel-StackTrace -d perl-Class-Data-Inheritable -d perl-Convert-ASN1 -d perl-Exception-Class -d perl-Compress-Raw-Zlib -d perl-Try-Tiny -d perl-Crypt-SSLeay -d perl-XML-NamespaceSupport -d perl-Archive-Zip \
 \
-x "**Devel/StackTrace**" -x "**Class/Data/Inheritable**" -x "**Convert/ASN1**" -x "**Exception/Class**" -x "**Compress/Raw/Zlib**" -x "**Try/Tiny**" -x "**Crypt/SSLeay**" -x "**XML/NamespaceSupport**" -x "**Archive/Zip**" \
 \
/etc/vmware-vcli /opt/vmwarecli /usr/lib64/perl5/auto/Locale /usr/lib64/perl5/auto/Params /usr/lib64/perl5/perllocal.pod /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/share/perl5/Locale/Maketext /usr/share/perl5/Params /usr/share/perl5/VMware /usr/share/perl5/WSMan
```

Jackpot. None of the replaced modules are in the RPM, but it did reveal something else…

VMware didn’t track the dependencies that CPAN built, only the ones it needed. What do I mean? For example, /usr/local/lib64/perl5/Class/MethodMaker.pm appears in our RPM bundle and our final.list. It was installed while vmware-cli was installing. but there is no trace of Class::MethodMaker in /etc/vmware-vcli/locations.

That means there’s an entirely new group of CPAN modules that exist that we probably don’t need. We’ll put this on the back burner and circle back later (maybe). In the meantime we need to switch over to our Raw snapshot and test this new RPM. Make sure to copy it to your local machine before rebooting.

Once it’s copied over, try installing it again:

```
yum install /tmp/vmware-cli-6.0.0_2503617-1.noarch.rpm
<...>
Install 1 Package (+10 Dependent packages)
<...>
```

It should be our friendly new RPMs.

```
/opt/vmwarecli/bin/vmware-cmd --server vcenter.domain.com -l -h esxhost1 --username vcenteruser@domain.com --password 'somethingsecure'
/vmfs/volumes/54eaad0a-923ab37c-90b6-b82a72d4d796/hostname1/hostname1.vmx
/vmfs/volumes/54eaad24-7c233c62-970e-b82a72d4d796/hostname2/hostname2.vmx
<...>
```

SUCCESS!

At this point, our RPM is installing the following CPAN modules (based on packlist files):

```
Locale::Maketext::Simple
Params::Check
Class::Inspector
Class::MethodMaker
Compress::Raw::Bzip2
Crypt::OpenSSL::RSA
Crypt::OpenSSL::Random
Crypt::X509
Data::Dump
Devel::CheckLib
Digest::MD5
Encode::Locale
Env
ExtUtils::CBuilder
ExtUtils::MakeMaker
File::Listing
HTML::Parser
HTML::Tagset
HTTP::Cookies
HTTP::Daemon
HTTP::Date
HTTP::Message
HTTP::Negotiate
IO::CaptureOutput
IO::Compress
IO::HTML
IO::SessionData
IO::Socket::INET6
IO::Socket::SSL
IPC::Cmd
Import::Into
JSON::PP
LWP
LWP::MediaTypes
LWP::Protocol::https
Module::Build
Module::CoreList
Module::Load
Module::Load::Conditional
Module::Metadata
Module::Runtime
Mozilla::CA
Net::HTTP
Net::INET6Glue
Net::SSLeay
Path::Class
Perl::OSType
SOAP::Lite
Socket6
Sub::Uplevel
Task::Weaken
Test::Simple
Test::Warn
URI
UUID
UUID::Random
WWW::RobotRules
XML::LibXML
XML::Parser::Lite
XML::SAX
XML::SAX::Base
autodie
version
```

I’m positive we could trim out many if not most of these, but I’m exhausted. I still need to get the rest of my stuff installed.

If this was helpful, please leave a comment below.