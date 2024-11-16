---
author: Jesse Morgan
categories:
- Uncategorized
date: "2015-05-20T08:06:37Z"
guid: http://morgajel.net/?p=1676
id: 1676
title: The Pain and Fury of vmware-cli on CentOS 7
url: /2015/05/20/1676
---

ok, this has reached the point of absurdity that I need to document it.

## The Goal

Install vmware-cli on CentOS 7 for check\_vmware\_esx.pl to use.

## The Requirements

- installation needs to be reproducible with ansible.
- the latest version of the cli, VMware-vSphere-CLI-6.0.0-2503617.x86\_64.tar.gz

## The Complications

1. CentOS 7 uses a new version of Perl that is binary incompatible.
2. VMware doesn’t pay attention.
3. CPAN seemed like a great idea 22 years ago. Unfortunately it didn’t keep up with the times.

## The Setup

- Freshly installed CentOS 7
- The following custom ansible packages are installed (to give you an idea of what’s I was starting with there without absurd detail): 
    - vim
    - sudo
    - sshkeys
    - bashconfig
    - snmpd
    - mariadb
    - httpd
    - git
    - icinga2(nagios-plugins,nagios-plugins-all )
    - manubulonplugins (perl-Net-SNMP)
    - morgnagplug (perl-HTML-Parser,perl-Compress-Raw-Zlib,perl-IO-Zlib,perl-libxml-perl,perl-XML-LibXML,perl-Time-Duration,perl-Number-Format,perl-Config-IniFiles,perl-DateTime)
- CPAN is NOT installed (it’s a PITA with ansible, so it’s a last resort).

I have currently have two snapshots:

- a barebones snapshot without all this stuff installed (but it takes 20 minutes to reinstall it all). (Raw Snapshot)
- a snapshot with all of the stuff listed above installed. (Configured Snapshot)

## Test 1: Simplest possible install using –default

The initial installation demands openssl-devel be installed (I’m skipping that hassle for brevity).

```
yum install openssl-devel
./vmware-install.pl --prefix=/opt/vmwarecli EULA_AGREED=yes --default
```

This will select the perl modules pre-packaged by VMware. Note that it does complain about the following:

```
MIME::Base64 3.14 or newer 
Try::Tiny 0.22 or newer 
Socket6 0.23 or newer
```

Which are installed via RPM for net-snmp and other packages and are non-negotiable. It installs fine, except it doesn’t work:

```
 /opt/vmwarecli/bin/vmware-cmd --server vcenter.domain.com -l -h esxhost1 --username vcenteruser@domain.com --password 'somethingsecure'
Hiding the command line arguments: symbol lookup error: /usr/lib64/perl5/auto/Crypt/SSLeay/SSLeay.so: undefined symbol: Perl_Gthr_key_ptr
```

This is where Complication #1 comes into play- SSLeay.so was compiled against a different system, so it’s incompatible. That’s a simple fix, right?

```
./bin/vmware-uninstall-vSphere-CLI.pl
yum install perl-Crypt-SSLeay
./vmware-install.pl --prefix=/opt/vmwarecli EULA_AGREED=yes --default
/opt/vmwarecli/bin/vmware-cmd --server vcenter.domain.com -l -h esxhost1 --username vcenteruser@domain.com --password 'somethingsecure'
 length() used on @array (did you mean "scalar(@array)"?) at /usr/lib64/perl5/IO/Compress/Zlib/Extra.pm line 198. SOAP request error - possibly a protocol issue: 500 read timeout
```

out of the frying pan and into the fire. There’s two actual problems here:

1. The pre-packaged modules include a broken copy of IO::Compress::Zlib. This is annoying, but it doesn’t stop anything (I don’t think).
2. There’s a problem with reading the soap stuff. what problem? Not sure yet.

That SOAP error first appears in 2009. it ties back to an old version of LWP (aka perl-libwww-perl.rpm). This is installed because morgnagplug uses perl-XML-LibXML.

OK, fine. I can break morgnagplug for the time being (testing purposes), so I remove that and rerun the installer. This pulls down the vmware-cli pre-packaged version of LWP and libxml (note that perl-libxml-perl.noarch.rpm is still installed through all of this). I’ve also removed it from my

```
/opt/vmwarecli/bin/vmware-cmd --server vcenter.domain.com -l -h esxhost1 --username vcenteruser@domain.com --password 'somethingsecure'
/usr/bin/perl: symbol lookup error: /usr/lib64/perl5/auto/XML/LibXML/Common/Common.so: undefined symbol: Perl_Gthr_key_ptr
```

So Now I’m at an impasse- two of the vmware prepackaged modules are not binary compatible- while I could work around Crypt::SSLeay, I couldn’t work around XML::LibXML because it’s dependent on perl-libwww-perl (LWP), which is incompatible.

At this point I can attempt to track down older versions of perl-libwww-perl, but that will then require older RPMs of half a dozen other libraries. This will require pinning them so they’re not accidentally upgraded and ignoring any security patches that may appear. Obviously this is not acceptable.

I don’t think I can go any further down this road. Time to consider CPAN.

## Test 2: Maximum CPAN

I roll back to my Configured snapshot, then install openssl-devel like before, then cpan and friends. This includes libuuid-devel for UUID and gcc (!) to get everything CPAN wants to install first try. Note this was about 3 days of trial and error to figure out what was missing so that everything would install properly (mostly because of UUID).

```
yum install openssl-devel libuuid-devel perl-YAML perl-Devel-CheckLib gcc perl-CPAN
```

Just as an FYI, CPAN is dependent on perl-ExtUtils-Install, **perl-ExtUtils-MakeMaker,** perl-ExtUtils-Manifest, perl-ExtUtils-ParseXS, perl-Test-Harness, perl-devel, and perl-local-lib among other things. This is important in a moment (Note: RPM installed perl-ExtUtils-MakeMaker-6.68-3.el7.noarch)

```
./vmware-install.pl --prefix=/opt/vmwarecli EULA_AGREED=yes
<...>
Do you want to install precompiled Perl modules for RHEL?
[yes] no
<...>
Can't locate Module/Build.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .).
BEGIN failed--compilation aborted.
Below mentioned modules with their version needed to be installed,
these modules are available in your system but vCLI need specific 
version to run properly

Module: ExtUtils::MakeMaker, Version: 6.96 
Module: Module::Build, Version: 0.4205 
Module: LWP, Version: 5.837 
Do you want to continue? (yes/no)
```

Note that the compilation failure above is due to a missing Module::Build, which it looks like it’ll install below anyways. Also note the three packages that need explicit versions-

- perl-ExtUtils-MakeMaker 6.68 **vs** ExtUtils::MakeMaker 6.96 (**Newer** than RPM)
- perl-Module-Build 0.40.05 **vs** Module::Build 0.4205 (**Newer** than RPM)
- perl-libwww-perl 6.05 **vs** LWP 5.837 (**Older** than RPM)

In either case, this fails because Class::MethodMaker can’t be built and the script fails:

```
CPAN not able to install following Perl modules on the system. These must be 
installed manually for use by vSphere CLI:

Class::MethodMaker 2.10 or newer
```

Following it’s directions, I try to install it manually to see what it’s missing:

```
 cpan -i Class::MethodMaker
Reading '/root/.cpan/Metadata'
 Database was generated on Tue, 19 May 2015 16:17:02 GMT
Running install for module 'Class::MethodMaker'
Running make for S/SC/SCHWIGON/class-methodmaker/Class-MethodMaker-2.24.tar.gz
Checksum for /root/.cpan/sources/authors/id/S/SC/SCHWIGON/class-methodmaker/Class-MethodMaker-2.24.tar.gz ok

 CPAN.pm: Building S/SC/SCHWIGON/class-methodmaker/Class-MethodMaker-2.24.tar.gz

Checking if your kit is complete...
Looks good
JSON::PP 2.27103 is not available
 at /usr/local/share/perl5/CPAN/Meta/Converter.pm line 22.
 at /usr/local/share/perl5/ExtUtils/MM_Any.pm line 831.
JSON::PP 2.27103 is not available
 at /usr/local/share/perl5/CPAN/Meta/Converter.pm line 22.
Warning: No success on command[/usr/bin/perl Makefile.PL]
 SCHWIGON/class-methodmaker/Class-MethodMaker-2.24.tar.gz
 /usr/bin/perl Makefile.PL -- NOT OK
Running make test
 Make had some problems, won't test
Running make install
 Make had some problems, won't install
Could not read metadata file. Falling back to other methods to determine prerequisites
```

So CPAN fails because it requires JSON::PP; I have three choices- install JSON::PP via CPAN, install JSON::PP via RPM or install Class::MethodMaker via RPM. I don’t want to get into dependency hell with cpan, and I know from previous experience that it will complain that the rpm for JSON::PP is too old, so I’ll attempt to simply install the perl-Class-MethodMaker rpm (which oddly isn’t dependent on JSON::PP). I’ll add this to my “always install first” list with openssl-devel.

(This is especially frustrating because I \*know\* that the installer will eventually use cpan to install JSON::PP, and this error is likely caused by poor dependency ordering.)

This works, and I’m able to run the install script again; this time with only two problematic modules:

```
The following Perl modules were found on the system but may be too old to work 
with vSphere CLI:

MIME::Base64 3.14 or newer 
Socket6 0.23 or newer
```

The real question- will vmware-cmd work?

```
/opt/vmwarecli/bin/vmware-cmd --server vcenter.domain.com -l -h esxhost1 --username vcenteruser@domain.com --password 'somethingsecure'

```

And that’s a no-go. It’ll hang for 5 minutes or so before erroring. I can tell 20 seconds in that this is the case, but for certainty I have to wait for the timeout before I can continue. What I get is even more interesting:

```
SOAP request error - possibly a protocol issue: <?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope xmlns:soapenc="http://schemas.xmlsoap.org/soap/encoding/"
<GIGANTIC BLOCK OF XML>
```

Well what the hell. 4 screens full of solid xml code. Looking over it I see snippets like this:

```
<summary>ISO [Datastore 1] ISOs/SW_DVD9_Windows_Svr_Std_and_DataCtr_2012_R2_64Bit_English_-4_MLF_X19-82891.ISO</summary>
```

Which means at least it’s getting through.

So I google for that error and find [this page](https://communities.vmware.com/message/2298661), which indicates Net::HTTP needs to be an older version just like LWP. The thing is I’m not intentionally installing the perl-Net-HTTP RPM; it’s installed by morgnagplug along with perl-XML-LibXML, perl-XML-SAX and perl-XML-LibXML… so I guess I uninstall those four RPMs and rerun the installer? This is getting confusing. I suspect I should reimage this machine without morgnagplug and see if that makes things easier. I’ll save that for the next attempt.

```
yum remove perl-Net-HTTP perl-XML-LibXML perl-XML-SAX perl-libwww-perl
./vmware-install.pl --prefix=/opt/vmwarecli EULA_AGREED=yes
<...>
CPAN not able to install following Perl modules on the system. These must be 
installed manually for use by vSphere CLI:

XML::LibXML::Common 0.13 or newer 
XML::LibXML 1.63 or newer
```

\*Sigh\*

```
cpan -i XML::LibXML
<...>
Checking for ability to link against libxml2...libxml2, zlib, and/or the Math library (-lm) have not been found.
Try setting LIBS and INC values on the command line
Or get libxml2 from
 http://xmlsoft.org/
If you install via RPMs, make sure you also install the -devel
RPMs, as this is where the headers (.h files) are.

Also, you may try to run perl Makefile.PL with the DEBUG=1 parameter
to see the exact reason why the detection of libxml2 installation
failed or why Makefile.PL was not able to compile a test program.
No 'Makefile' created SHLOMIF/XML-LibXML-2.0121.tar.gz
<...>
```

If I read that correctly, cpan won’t build it because the libxml2-devel rpm is not installed.

```
yum install libxml2-devel.x86_64
cpan -i XML::LibXML
<...>
Warning: prerequisite XML::SAX 0.11 not found.
JSON::PP 2.27103 is not available
 at /usr/local/share/perl5/CPAN/Meta/Converter.pm line 22.
 at /usr/local/share/perl5/ExtUtils/MM_Any.pm line 831.
JSON::PP 2.27103 is not available
 at /usr/local/share/perl5/CPAN/Meta/Converter.pm line 22.
Warning: No success on command[/usr/bin/perl Makefile.PL]
 SHLOMIF/XML-LibXML-2.0121.tar.gz
 /usr/bin/perl Makefile.PL -- NOT OK
<...>
```

damnit. Looks like I’m going down the CPAN rabbit hole.

```
cpan -i JSON::PP
cpan -i XML::SAX
<...>
t/21saxini.t ..... Can't locate Fatal.pm in @INC (@INC contains: /root/.cpan/build/XML-SAX-0.99-OJOmqR/blib/lib /root/.cpan/build/XML-SAX-0.99-OJOmqR/blib/arch /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at t/21saxini.t line 6.
BEGIN failed--compilation aborted at t/21saxini.t line 6.
t/21saxini.t ..... Dubious, test returned 2 (wstat 512, 0x200)
No subtests run 
t/40cdata.t ...... ok 
t/42entities.t ... ok 
t/99cleanup.t .... ok 

Test Summary Report
-------------------
t/21saxini.t (Wstat: 512 Tests: 0 Failed: 0)
 Non-zero exit status: 2
 Parse errors: No plan found in TAP output
Files=14, Tests=98, 2 wallclock secs ( 0.11 usr 0.02 sys + 2.07 cusr 0.16 csys = 2.36 CPU)
Result: FAIL
Failed 1/14 test programs. 0/98 subtests failed.
make: *** [test_dynamic] Error 255
 GRANTM/XML-SAX-0.99.tar.gz
 /bin/make test -- NOT OK
//hint// to see the cpan-testers results for installing this module, try:
 reports GRANTM/XML-SAX-0.99.tar.gz
Running make install
 make test had returned bad status, won't install without force
```

what? Fine.

```
cpan -i Fatal
cpan -i XML::SAX
cpan -i XML::LibXML
```

FINALLY.

ok… where was I? Oh yes.

```
./vmware-install.pl --prefix=/opt/vmwarecli EULA_AGREED=yes
```

It runs and complains about MIME::Base64 and Socket6 again.

FINGERS CROSSED…

```
/opt/vmwarecli/bin/vmware-cmd --server vcenter.domain.com -l -h esxhost1 --username vcenteruser@domain.com --password 'somethingsecure'
SOAP request error - possibly a protocol issue: <?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope xmlns:soapenc="http://schemas.xmlsoap.org/soap/encoding/"
<...>
```

and it takes forever again… not a good sign… aaand another giant block of xml.

Searching again finds [this page](http://www.opsview.com/forum/opsview-core/latest-release/core-virtual-appliance-and-vmware-perl-lib-55).

Again pointing out that Net::HTTP is the source of the problem…. but I uninstalled the bad version and let the installer install the correct one, right?

```
 perl -MNet::HTTP -le 'print $Net::HTTP::VERSION'
6.07
perl -e 'use LWP; print LWP->VERSION."\n"'
6.13
```

WAT?

OK… so lets install specific friggen version manually. the installer mentions LWP 5.837

```
cpan -i GAAS/libwww-perl-5.837.tar.gz
perl -MNet::HTTP -le 'print $Net::HTTP::VERSION'
5.834
perl -e 'use LWP; print LWP->VERSION."\n"'
5.837
```

ok, 17th try is a charm…

```
/opt/vmwarecli/bin/vmware-cmd --server vcenter.domain.com -l -h esxhost1 --username vcenteruser@domain.com --password 'somethingsecure'
/vmfs/volumes/54aead0a-9231b37c-90a6-b82a72d4d796/hostname1/hostname1.vmx
/vmfs/volumes/54aead24-7c23cb62-980e-b82a72d4d796/hostname2/hostname2.vmx
<...>
```

WOOOOOOOOOOOOO. IT FINALLY WORKS!

… so you remember how we did that, right?

It’s SOOOO tempting to throw my hand up, say done, and walk away, but it doesn’t fit our first requirement- reproducible from ansible.

Remember, you can’t use \*cpan\* from ansible. Sure, you can use cpanm, but not cpan.

Now that I roughly know what I need, I’m going to go back to the Raw Snapshot capture the state of all my perl module directories, reinstall vmware-cli, then bundle them all into a single RPM with FPM. Why? Because CPAN will not be needed at that point, and I will be able to rebuild that state much easier.

## Test 3: Clean Slate and FPM

Clean slate sucks because I don’t have sudo set up along with a slew of other useful things. Oh well. First things first- lets get a list of all of our current perl modules

```
perl -e 'use doesntexist;'
Can't locate doesntexist.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at -e line 1.
BEGIN failed--compilation aborted at -e line 
find /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 >/tmp/pristine.list
```

Some of those dirs don’t even exist yet, but it’s still important to track them.

I copy the vmware folder back over, and begin installing all of my prereqs. At this point it’s only a few things:

```
yum install openssl-devel
yum install libuuid-devel perl-YAML perl-Devel-CheckLib gcc perl-CPAN libxml2-devel.x86_64
```

Next we’re gonna run the installation script. I’m pretty sure that it’ll fail, but at least it’ll configure CPAN the way it needs it rather than me bumbling through it on the first run.

```
./vmware-install.pl --prefix=/opt/vmwarecli EULA_AGREED=yes
<...>
Do you want to install precompiled Perl modules for RHEL?
[yes] no
<...>

Can't locate Module/Build.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .).
BEGIN failed--compilation aborted.
Can't locate LWP.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .).
BEGIN failed--compilation aborted.
Below mentioned modules with their version needed to be installed,
these modules are available in your system but vCLI need specific 
version to run properly

Module: ExtUtils::MakeMaker, Version: 6.96 
Module: Module::Build, Version: 0.4205 
Module: LWP, Version: 5.837 
Do you want to continue? (yes/no) yes
<...>

CPAN not able to install following Perl modules on the system. These must be 
installed manually for use by vSphere CLI:

Class::MethodMaker 2.10 or newer
```

OK, now that CPAN is installed, I should be able to simply install these two:

```
cpan -i JSON::PP
cpan -i Fatal
cpan -i Class::MethodMaker
<...>
PERL_DL_NONLAZY=1 /usr/bin/perl "-MExtUtils::Command::MM" "-MTest::Harness" "-e" "undef *Test::Harness::Switches; test_harness(0, 'blib/lib', 'blib/arch')" t/*.t
t/array.t .............. Can't locate Env.pm in @INC (@INC contains: /root/.cpan/build/Class-MethodMaker-2.24-hloQDv/t /root/.cpan/build/Class-MethodMaker-2.24-hloQDv/blib/lib /root/.cpan/build/Class-MethodMaker-2.24-hloQDv/blib/arch /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at /root/.cpan/build/Class-MethodMaker-2.24-hloQDv/t/test.pm line 135.
BEGIN failed--compilation aborted at /root/.cpan/build/Class-MethodMaker-2.24-hloQDv/t/test.pm line 135.
Compilation failed in require at t/array.t line 22.
BEGIN failed--compilation aborted at t/array.t line 22.
t/array.t .............. Dubious, test returned 2 (wstat 512, 0x200)
<...>
Files=8, Tests=0, 1 wallclock secs ( 0.04 usr 0.00 sys + 0.44 cusr 0.06 csys = 0.54 CPU)
Result: FAIL
Failed 8/8 test programs. 0/0 subtests failed.
make: *** [test_dynamic] Error 2
 SCHWIGON/class-methodmaker/Class-MethodMaker-2.24.tar.gz
 /bin/make test -- NOT OK
//hint// to see the cpan-testers results for installing this module, try:
 reports SCHWIGON/class-methodmaker/Class-MethodMaker-2.24.tar.gz
Running make install
 make test had returned bad status, won't install without force


```

Oh FFS. Maybe there’s an Env package I’m unfamiliar with that’s needed? I thought that was part of perl core… apparently not.

```
cpan -i Env
cpan -i Class::MethodMaker
```

and rerun the installer again…

```
./vmware-install.pl --prefix=/opt/vmwarecli EULA_AGREED=yes
<...>
Do you want to install precompiled Perl modules for RHEL?
[yes] no
<...>
```

SUCCESS.

```
/opt/vmwarecli/bin/vmware-cmd --server vcenter.domain.com -l -h esxhost1 --username vcenteruser@domain.com --password 'somethingsecure'
/vmfs/volumes/54eaad0a-923ab37c-90b6-b82a72d4d796/hostname1/hostname1.vmx
/vmfs/volumes/54eaad24-7c233c62-970e-b82a72d4d796/hostname2/hostname2.vmx
<...>
```

HURRAY IT WORKS!

Make a quick Snapshot to give us a recovery point.

## Cleaning up

While there are many things we need to clean up, there’s one thing I want to get out of the way quickly- removing some of the cruft we installed to get CPAN to work. Unlike Debian’s apt, yum doesn’t notify you when dependencies are no longer needed, so I browed /var/log/yum.log and reviewed all of the packages installed while we were doing this. Here’s the list I came up with:

```
yum remove libcom_err-devel libsepol-devel zlib-devel keyutils-libs-devel pcre-devel libselinux-devel libverto-devel krb5-devel openssl-devel mpfr libmpc cpp kernel-headers glibc-headers glibc-devel libdb-devel perl-local-lib perl-Digest perl-Digest-SHA gdbm-devel perl-Test-Harness pyparsing systemtap-sdt-devel perl-ExtUtils-Manifest perl-ExtUtils-Install perl-ExtUtils-MakeMaker perl-ExtUtils-ParseXS perl-devel perl-CPAN gcc perl-YAML libuuid-devel perl-Devel-CheckLib xz-devel libxml2-devel
```

After removing that, we

```
/opt/vmwarecli/bin/vmware-cmd --server vcenter.domain.com -l -h esxhost1 --username vcenteruser@domain.com --password 'somethingsecure'
/vmfs/volumes/54eaad0a-923ab37c-90b6-b82a72d4d796/hostname1/hostname1.vmx
/vmfs/volumes/54eaad24-7c233c62-970e-b82a72d4d796/hostname2/hostname2.vmx
<...>
```

Excellent- everything seems to be working.

Lets take one last snapshot of our perl modules for later comparison to pristine.list:

```
find /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 >/tmp/final.list
```

We now have a functioning vmware-cmd, but it STILL doesn’t meet our primary requirement- it must work via ansible.

We’ll pick up the rest in [Part 2](http://morgajel.net/2015/05/20/1681).