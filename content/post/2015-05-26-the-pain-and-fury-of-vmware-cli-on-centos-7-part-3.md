---
author: Jesse Morgan
categories:
- Uncategorized
date: "2015-05-26T11:11:03Z"
guid: http://morgajel.net/?p=1696
id: 1696
title: The Pain and Fury of vmware-cli on CentOS 7, Part 3
url: /2015/05/26/1696
---

So a bit of followup since my last post; I’ve begun reinstalling icinga and my other plugins and have already ran into some resistance- perl-net-SNMP is required by morgnagplug, and has a dependency on perl-Socket6, which conflicts with the vmware-cli rpm.

## The Plan

I’m going to do the following:

1. snapshot my current “production-run” version of the server
2. jump back to my “package-created” snapshot
3. add modules listed below to the exclude list
4. rebuild the RPM
5. save newest RPM locally
6. revert to the raw snapshot
7. upload the newest version of the RPM
8. install it
9. test vmware-cmd
10. If I get an error, install corresponding RPM, then repeat until it runs properly. 
    1. If this doesn’t work, revert back to the package-created snapshot
    2. exclude all EXCEPT the troublesome module
    3. rebuild RPM and repeat test.

In addition to Socket6, I will attempt to replace at the same time:

- Class::MethodMaker with perl-Class-MethodMaker
- Compress::Raw::Bzip2 with perl-Compress-Raw-Bzip2
- Devel::CheckLib with perl-Devel-CheckLib
- Encode::Locale with perl-Encode-Locale
- Env with perl-Env
- IO::HTML with perl-IO-HTML
- Import::Into with perl-Import-Into
- IO::Socket::INET6 with perl-IO-Socket-INET6.noarch
- Locale::Maketext::Simple with perl-Locale-Maketext-Simple and perl-Locale-Maketext
- Mozilla::CA with perl-Mozilla-CA
- Net::SSLeay with perl-Net-SSLeay
- Perl::OSType with perl-Perl-OSType
- Params::Check with perl-Params-Check
- Path::CLass with perl-Path-Class
- Socket6 with perl-Socket6
- Sub::Uplevel with perl-Sub-Uplevel
- Task::Weaken with perl-Task-Weaken
- Test::Warn with perl-Test-Warn
- autodie with perl-autodie
- version with perl-version

Once I’ve reached a stable point where I can install the newest RPM and it’s dependencies:

- I’ll rebuild the module with the proper excludes and add the new dependence
- copy it locally
- revert to my “production-run” snapshot
- uninstall the old RPM
- install the new RPM
- verify the new dependencies install
- verify vmware-cmd works as well as check\_vmware\_esx.pl

It’s also worth nothing that I’ll be updating FPM to use –iteration 2 since this is such a big departure from the last one.

## The Implementation

Reverted back to package-created snapshot

```
fpm -f --post-install /tmp/vmlinker.sh -s dir -d perl-Data-Dumper -a noarch --rpm-user root --rpm-group root -t rpm -n vmware-cli -v '6.0.0_2503617' --iteration=2 -C / \
 \
-d perl-Devel-StackTrace -d perl-Class-Data-Inheritable -d perl-Convert-ASN1 -d perl-Exception-Class -d perl-Compress-Raw-Zlib -d perl-Try-Tiny -d perl-Crypt-SSLeay -d perl-XML-NamespaceSupport -d perl-Archive-Zip \
 \
-x "**Devel/StackTrace**" -x "**Class/Data/Inheritable**" -x "**Convert/ASN1**" -x "**Exception/Class**" -x "**Compress/Raw/Zlib**" -x "**Try/Tiny**" -x "**Crypt/SSLeay**" -x "**XML/NamespaceSupport**" -x "**Archive/Zip**" \
 \
-x "**Class/MethodMaker**" -x "**Devel/CheckLib**" -x "**Compress/Raw/Bzip2**" -x "**Encode/Locale**" -x "**Env**" -x "**IO/HTML**" -x "**Import/Into**" -x "**IO/Socket/INET6**" -x "**Locale/Maketext/Simple**" -x "**Mozilla/CA**" -x "**Net/SSLeay**" -x "**Perl/OSType**" -x "**Params/Check**" -x "**Path/CLass**" -x "**Socket6**" -x "**Sub/Uplevel**" -x "**Task/Weaken**" -x "**Test/Warn**" -x "**autodie**" -x "**version**" \
 \
/etc/vmware-vcli /opt/vmwarecli /usr/lib64/perl5/auto/Locale /usr/lib64/perl5/auto/Params /usr/lib64/perl5/perllocal.pod /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/share/perl5/Locale/Maketext /usr/share/perl5/Params /usr/share/perl5/VMware /usr/share/perl5/WSMan
```

As I typed that out, I became nervous about those regexex- specifically version and autodie- they felt too generic.

I confirmed my fears by comparing the filelist from iteration 1 and iteration 2:

```
rpm -qlp vmware-cli-6.0.0_2503617-1.noarch.rpm >/tmp/1
rpm -qlp vmware-cli-6.0.0_2503617-2.noarch.rpm >/tmp/2
vimdiff /tmp/1 /tmp/2
```

That sloppy regex got rid of /opt/vmwarecli/lib/vmware-vcli/apps/general/viversion.pl, which I did NOT want.

Inserting a slash into that wildcart to change it to -x “\*\*/version\*\*” seemed to do the trick, but it made me curious what else I’d missed. I decided to remove ALL of the excludes and package it as iteration 0, then compare it to my new iteration 2 (with \*\*/version\*\*):

```
fpm -f --post-install /tmp/vmlinker.sh -s dir -d perl-Data-Dumper -a noarch --rpm-user root --rpm-group root -t rpm -n vmware-cli -v '6.0.0_2503617' --iteration=0 -C / \
 \
-d perl-Devel-StackTrace -d perl-Class-Data-Inheritable -d perl-Convert-ASN1 -d perl-Exception-Class -d perl-Compress-Raw-Zlib -d perl-Try-Tiny -d perl-Crypt-SSLeay -d perl-XML-NamespaceSupport -d perl-Archive-Zip \
 \
/etc/vmware-vcli /opt/vmwarecli /usr/lib64/perl5/auto/Locale /usr/lib64/perl5/auto/Params /usr/lib64/perl5/perllocal.pod /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/share/perl5/Locale/Maketext /usr/share/perl5/Params /usr/share/perl5/VMware /usr/share/perl5/WSMan


fpm -f --post-install /tmp/vmlinker.sh -s dir -d perl-Data-Dumper -a noarch --rpm-user root --rpm-group root -t rpm -n vmware-cli -v '6.0.0_2503617' --iteration=2 -C / \
 \
-d perl-Devel-StackTrace -d perl-Class-Data-Inheritable -d perl-Convert-ASN1 -d perl-Exception-Class -d perl-Compress-Raw-Zlib -d perl-Try-Tiny -d perl-Crypt-SSLeay -d perl-XML-NamespaceSupport -d perl-Archive-Zip \
 \
-x "**Devel/StackTrace**" -x "**Class/Data/Inheritable**" -x "**Convert/ASN1**" -x "**Exception/Class**" -x "**Compress/Raw/Zlib**" -x "**Try/Tiny**" -x "**Crypt/SSLeay**" -x "**XML/NamespaceSupport**" -x "**Archive/Zip**" \
 \
-x "**Class/MethodMaker**" -x "**Devel/CheckLib**" -x "**Compress/Raw/Bzip2**" -x "**Encode/Locale**" -x "**Env**" -x "**IO/HTML**" -x "**Import/Into**" -x "**IO/Socket/INET6**" -x "**Locale/Maketext/Simple**" -x "**Mozilla/CA**" -x "**Net/SSLeay**" -x "**Perl/OSType**" -x "**Params/Check**" -x "**Path/CLass**" -x "**Socket6**" -x "**Sub/Uplevel**" -x "**Task/Weaken**" -x "**Test/Warn**" -x "**autodie**" -x "**/version**" \
 \
/etc/vmware-vcli /opt/vmwarecli /usr/lib64/perl5/auto/Locale /usr/lib64/perl5/auto/Params /usr/lib64/perl5/perllocal.pod /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/share/perl5/Locale/Maketext /usr/share/perl5/Params /usr/share/perl5/VMware /usr/share/perl5/WSMan

rpm -qlp vmware-cli-6.0.0_2503617-0.noarch.rpm >/tmp/0
rpm -qlp vmware-cli-6.0.0_2503617-2.noarch.rpm >/tmp/2
vimdiff /tmp/0 /tmp/2
```

The first thing I noticed was that while I excluded Locale/Maketext/Simple, it did not remove the Local/Maketext directory, leaving it empty. I dislike including empty directories, but it’s relatively minor. Perhaps I’ll explicitly remove them in a future iteration.

Next:

- copied iteration2 of the RPM to my local machine
- time to revert to the Raw snapshot.
- upload iteration2
- yum install rpm with it’s original 10 dependencies.
- test vmware-cmd

```
/opt/vmwarecli/bin/vmware-cmd --server vcenter.domain.com -l -h esxhost1 --username vcenteruser@domain.com --password 'somethingsecure'
/vmfs/volumes/54eaad0a-923ab37c-90b6-b82a72d4d796/hostname1/hostname1.vmx
/vmfs/volumes/54eaad24-7c233c62-970e-b82a72d4d796/hostname2/hostname2.vmx
<...>
```

By some miracle, it works! This means that all of the packages we’ve excluded are not actually needed (at least for vmware-cmd). Our final test is to bounce back to our “production run” snapshot and swap out the iteration1 RPM for iteration2.

```
yum remove vmware-cli
yum install vmware-cli-6.0.0_2503617-2.noarch.rpm 
<...>
ln: failed to access ‘/usr/bin/dcli’: Too many levels of symbolic links
ln: failed to access ‘/usr/bin/esxcfg-advcfg’: Too many levels of symbolic links
ln: failed to access ‘/usr/bin/esxcfg-authconfig’: Too many levels of symbolic links
ln: failed to access ‘/usr/bin/esxcfg-cfgbackup’: Too many levels of symbolic links
ln: failed to access ‘/usr/bin/esxcfg-dns’: Too many levels of symbolic links
ln: failed to access ‘/usr/bin/esxcfg-dumppart’: Too many levels of symbolic links
ln: failed to access ‘/usr/bin/esxcfg-hostops’: Too many levels of symbolic links
<...>
```

Whoops. Remember that vmlinker.sh script we made? It didn’t clean up after itself on uninstall. Yes, that’s sloppy on my part; I should include a pre-uninstall script to remove those symlinks. It turns out the links were broken anyways (damnit). If I have to repackage this RPM, I’ll include the uninstaller and fix the symlinks. For now, at least the package is installed. (I’ve also changed the example in the last article to function properly.)

The important thing is vmware-cmd is functional, as is check\_vmware\_esx.pl. Now to get all of my other checks functional.

One thing I am bothered by though. Despite not requiring that third round of dependencies, I still have a bunch from the first and second rounds:

```
-d perl-Data-Dumper
-d perl-Devel-StackTrace -d perl-Class-Data-Inheritable -d perl-Convert-ASN1 -d perl-Exception-Class -d perl-Compress-Raw-Zlib -d perl-Try-Tiny -d perl-Crypt-SSLeay -d perl-XML-NamespaceSupport -d perl-Archive-Zip \
```

The only one I’m \*sure\* I need (as in it failed without it) was perl-Data-Dumper; the rest I inferred. Perhaps I should verify those as well. Perhaps another time.

I should also consider another round at the CPAN modules left behind- things Digest::MD5 might not need to be in there.