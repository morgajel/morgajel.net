---
author: Jesse Morgan
categories:
- Uncategorized
date: "2015-05-28T13:18:43Z"
guid: http://morgajel.net/?p=1704
id: 1704
title: The Pain and Fury of vmware-cli on CentOS 7, Part 4
url: /2015/05/28/1704
---

Once more into the breach? Sure. Lets see what else we can remove, and destroy some dependencies as well. Our changelist includes:

- removing “-d perl-Devel-StackTrace -d perl-Class-Data-Inheritable -d perl-Convert-ASN1 -d perl-Exception-Class -d perl-Compress-Raw-Zlib -d perl-Try-Tiny -d perl-Crypt-SSLeay -d perl-XML-NamespaceSupport -d perl-Archive-Zip “
- adding -x “\*\*Digest/MD5\*\*” -x “\*\*Crypt/OpenSSL/RSA\*\*” -x “\*\*/Module/\*\*” -x “\*\*/Test/\*\*” -x “\*\*/ExtUtils/\*\*” \\
- updating iteration to 3
- Adding perl-Crypt-OpenSSL-RSA perl-Digest-MD5 as a tentative rpm dependencies
- removing symlinks via vmunlinker script

And here’s what I’m going to run.

```

echo '#!/bin/bash'> /tmp/vmunlinker.sh ; echo 'cd /opt/vmwarecli/bin/ ; for i in * ; do rm /usr/bin/$i ; done' >>/tmp/vmunlinker.sh
chmod 755 /tmp/vmunlinker.sh


fpm -f --pre-uninstall /tmp/vmunlinker.sh --post-install /tmp/vmlinker.sh -s dir -a noarch --rpm-user root --rpm-group root -t rpm -n vmware-cli -v '6.0.0_2503617' --iteration=3 -C / \
 \
-x "**Devel/StackTrace**" -x "**Class/Data/Inheritable**" -x "**Convert/ASN1**" -x "**Exception/Class**" -x "**Compress/Raw/Zlib**" -x "**Try/Tiny**" -x "**Crypt/SSLeay**" -x "**XML/NamespaceSupport**" -x "**Archive/Zip**" \
 \
-x "**Class/MethodMaker**" -x "**Devel/CheckLib**" -x "**Compress/Raw/Bzip2**" -x "**Encode/Locale**" -x "**Env**" -x "**IO/HTML**" -x "**Import/Into**" -x "**IO/Socket/INET6**" -x "**Locale/Maketext/Simple**" -x "**Mozilla/CA**" -x "**Net/SSLeay**" -x "**Perl/OSType**" -x "**Params/Check**" -x "**Path/CLass**" -x "**Socket6**" -x "**Sub/Uplevel**" -x "**Task/Weaken**" -x "**Test/Warn**" -x "**autodie**" -x "**/version**" \
-x "**Digest/MD5**" -x "**Crypt/OpenSSL/RSA**" -x "**/Module/**" -x "**/Test/**" -x "**/ExtUtils/**" \
 \
/etc/vmware-vcli /opt/vmwarecli /usr/lib64/perl5/auto/Locale /usr/lib64/perl5/auto/Params /usr/lib64/perl5/perllocal.pod /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/share/perl5/Locale/Maketext /usr/share/perl5/Params /usr/share/perl5/VMware /usr/share/perl5/WSMan


```

once that finishes, create a snapshot as “v3 built”, revert back to the raw build. I installed the RPM and:

- Confirmed “-d perl-Archive-Zip -d perl-Data-Dumper -d perl-Crypt-SSLeay” are the only dependencies needed to make vmware-cmd work.
- Confirmed symlinks worked

then removed rpm and:

- confirmed symlinks were removed

It seems like we have all of the relevant information. revert to “v3 built” and run this hopefully final command:

```
fpm -f --pre-uninstall /tmp/vmunlinker.sh --post-install /tmp/vmlinker.sh -s dir -a noarch --rpm-user root --rpm-group root -t rpm -n vmware-cli -v '6.0.0_2503617' --iteration=3 -C / \
 \
-d perl-Archive-Zip -d perl-Data-Dumper -d perl-Crypt-SSLeay \
-x "**Devel/StackTrace**" -x "**Class/Data/Inheritable**" -x "**Convert/ASN1**" -x "**Exception/Class**" -x "**Compress/Raw/Zlib**" -x "**Try/Tiny**" -x "**Crypt/SSLeay**" -x "**XML/NamespaceSupport**" -x "**Archive/Zip**" \
 \
-x "**Class/MethodMaker**" -x "**Devel/CheckLib**" -x "**Compress/Raw/Bzip2**" -x "**Encode/Locale**" -x "**Env**" -x "**IO/HTML**" -x "**Import/Into**" -x "**IO/Socket/INET6**" -x "**Locale/Maketext/Simple**" -x "**Mozilla/CA**" -x "**Net/SSLeay**" -x "**Perl/OSType**" -x "**Params/Check**" -x "**Path/CLass**" -x "**Socket6**" -x "**Sub/Uplevel**" -x "**Task/Weaken**" -x "**Test/Warn**" -x "**autodie**" -x "**/version**" \
-x "**Digest/MD5**" -x "**Crypt/OpenSSL/RSA**" -x "**/Module/**" -x "**/Test/**" -x "**/ExtUtils/**" \
 \
/etc/vmware-vcli /opt/vmwarecli /usr/lib64/perl5/auto/Locale /usr/lib64/perl5/auto/Params /usr/lib64/perl5/perllocal.pod /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/share/perl5/Locale/Maketext /usr/share/perl5/Params /usr/share/perl5/VMware /usr/share/perl5/WSMan

```

You now have your final product vmware-cli RPM.

Note that I have only been testing vmware-cmd; it’s possible the other 95% of the functionality may be borked.