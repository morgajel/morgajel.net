---
author: Jesse Morgan
categories:
- Hobbies
- Linux
- Open Source
date: "2006-08-25T22:15:47Z"
guid: http://morgajel.net/2006/08/25/162/
id: 162
title: 'Udev + wacom on gentoo: dynamic links'
url: /2006/08/25/162
views:
- "48"
---

I have a wacom drawing pad, and one thing that’s always bothered me was the whole way linux handled usb items- depending on the order they were plugged in, they would be given different names- sometimes my keyboard would be “/dev/input/event1”, sometimes my wacom or mouse.

Udev was made to get around these issues, but I’ve been to distracted to give it much thought. lately I’ve been getting the following error messages with my ruby projects:

```

X Error: BadDevice, invalid or uninitialized input device 165
  Major opcode:  147
  Minor opcode:  3
  Resource id:  0x0
Failed to open device
X Error: BadDevice, invalid or uninitialized input device 165
  Major opcode:  147
  Minor opcode:  3
  Resource id:  0x0
Failed to open device
```

and figure something screwy with my usb is the culprit. So with my sights set on an acceptable scapegoat, I delved into udev. I found a [great walkthrough for writing udev rules](http://reactivated.net/writing_udev_rules.html#example-usbcardreader) that gave me exactly what I needed. 20 minutes later, I had the following gem:

```

BUS=="usb", DRIVER="wacom", SYMLINK+="wacom"
```

That’s it. Now my wacom will always be /dev/wacom, and I can configure X to always use it. not sure if this will fix my error, but I’ve been meaning to do this for a while, so I’m glad I finally got around to it.