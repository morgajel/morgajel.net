---
author: Jesse Morgan
categories:
- Linux
- Open Source
date: "2007-09-02T18:11:59Z"
guid: http://morgajel.net/2007/09/02/219/
id: 219
title: Epson Perfection v350 on Ubuntu Feisty Fawn
url: /2007/09/02/219
views:
- "266"
---

Ok, I’ve done it twice now, I think I’ve figured it out.  
Get the source files from Epson’s linux website; I don’t recall the url but these are the files you need (or newer, no idea what the future holds):

```

iscan-2.3.0-1.c2.i386.rpm
iscan-plugin-gt-f700-2.0.0-0.c2.i386.rpm  
```

Use alien to convert them to debs (normally bad, but acceptable this time around).

Install both newly created deb packages with dpkg:  
`dpkg -i iscan_2.3.0-2_i386.deb iscan-plugin-gt-f700_2.0.0-1_i386.deb`  
Install sane, sane-utils:  
`<br></br>apt-get install sane sane-utils<br></br>`

Edit /etc/sane.d/dll.conf, replacing epson with epkowa (yes, I’m serious).

add the following to /etc/udev/rules.d/45-libsane.rules:  
`<br></br># Epson Perfection v350<br></br>SYSFS{idVendor}=="04b8", SYSFS{idProduct}=="012f", MODE="664", GROUP="scanner"<br></br>`

And I think that’s about it. Make sure you’re in the scanner group, and run:  
`<br></br>scanimage -L<br></br>`

That should simply list your scanner- if it doesn’t let me know- I might have missed something. From this point on you should be able to access the scanner with either sane or iscan.

The one downside to this driver is it doesn’t do the full 48000 PSI, only 24000- this means a scan of a guitar pick is \*only\* 16 inches across, rather than 32 inches 🙂 Unfortuantely it’s not the preferred resolution for negatives.