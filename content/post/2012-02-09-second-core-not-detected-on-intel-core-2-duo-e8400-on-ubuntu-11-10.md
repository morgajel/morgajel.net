---
author: Jesse Morgan
categories:
- Uncategorized
date: "2012-02-09T09:44:41Z"
guid: http://morgajel.net/?p=1184
id: 1184
title: Second Core not detected on Intel Core 2 Duo e8400 on Ubuntu 11.10
url: /2012/02/09/1184
---

Long title, but accurate. This was on a Dell Optiplex 760- dmesg showed only one core being found, cat /proc/cpu only showed one core, etc.

1. sudo vim /etc/default/grub
2. on GRUB\_CMDLINE\_LINUX\_DEFAULT, change acpi=off to acpi=noirq
3. save file
4. run sudo update-grub
5. reboot

Now your cores should be found. Leave a comment if this was able to help you.