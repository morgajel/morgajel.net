---
author: Jesse Morgan
categories:
- Uncategorized
date: "2015-03-01T11:09:32Z"
guid: http://morgajel.net/?p=1647
id: 1647
title: wacom Intuos tablet crashes Xorg in Kubuntu
url: /2015/03/01/1647
---

every 1 in 10 times I hook up my Wacom Intuos to Kubuntu, xorg crashes with this lovely message. It’s very irksome.

Linux linwider 3.16.0-28-generic #38-Ubuntu SMP Fri Dec 12 17:37:40 UTC 2014 x86\_64 x86\_64 x86\_64 GNU/Linux

Kubuntu 14.10

xsetwacom –list devices  
Wacom Intuos PT S Pen stylus id: 9 type: STYLUS  
Wacom Intuos PT S Finger touch id: 10 type: TOUCH  
Wacom Intuos PT S Pen eraser id: 16 type: ERASER  
Wacom Intuos PT S Finger pad id: 17 type: PAD

Bus 001 Device 005: ID 056a:0302 Wacom Co., Ltd

```
[180969.708] (II) config/udev: Adding input device Wacom Intuos PT S Pen (/dev/input/mouse2)
[180969.708] (II) No input driver specified, ignoring this device.
[180969.708] (II) This device may have been added with another device file.
[180969.710] (II) config/udev: Adding input device Wacom Intuos PT S Finger (/dev/input/event16)
[180969.710] (**) Wacom Intuos PT S Finger: Applying InputClass "evdev touchpad catchall"
[180969.710] (**) Wacom Intuos PT S Finger: Applying InputClass "touchpad catchall"
[180969.710] (**) Wacom Intuos PT S Finger: Applying InputClass "Default clickpad buttons"
[180969.710] (**) Wacom Intuos PT S Finger: Applying InputClass "Wacom class"
[180969.710] (II) Using input driver 'wacom' for 'Wacom Intuos PT S Finger'
[180969.710] (**) Wacom Intuos PT S Finger: always reports core events
[180969.710] (**) Option "Device" "/dev/input/event16"
[180969.710] (EE) Wacom Intuos PT S Finger: Invalid type 'stylus' for this device.
[180969.710] (EE) Wacom Intuos PT S Finger: Invalid type 'eraser' for this device.
[180969.710] (EE) Wacom Intuos PT S Finger: Invalid type 'cursor' for this device.
[180969.710] (II) Wacom Intuos PT S Finger: type not specified, assuming 'touch'.
[180969.710] (II) Wacom Intuos PT S Finger: other types will be automatically added.
[180969.710] (--) Wacom Intuos PT S Finger touch: maxX=4096 maxY=4096 maxZ=0 resX=26000 resY=43000 
[180969.710] (II) Wacom Intuos PT S Finger touch: hotplugging dependent devices.
[180969.710] (EE) Wacom Intuos PT S Finger touch: Invalid type 'stylus' for this device.
[180969.710] (EE) Wacom Intuos PT S Finger touch: Invalid type 'eraser' for this device.
[180969.710] (EE) Wacom Intuos PT S Finger touch: Invalid type 'cursor' for this device.
[180969.710] (II) Wacom Intuos PT S Finger touch: hotplugging completed.
[180969.760] (**) Option "config_info" "udev:/sys/devices/pci0000:00/0000:00:1a.0/usb1/1-1/1-1.2/1-1.2:1.1/input/input46/event16"
[180969.760] (II) XINPUT: Adding extended input device "Wacom Intuos PT S Finger touch" (type: TOUCH, id 14)
[180969.760] (**) Wacom Intuos PT S Finger touch: (accel) keeping acceleration scheme 1
[180969.760] (**) Wacom Intuos PT S Finger touch: (accel) acceleration profile 0
[180969.760] (**) Wacom Intuos PT S Finger touch: (accel) acceleration factor: 2.000
[180969.760] (**) Wacom Intuos PT S Finger touch: (accel) acceleration threshold: 4
[180969.760] (**) Wacom Intuos PT S Finger pad: Applying InputClass "evdev touchpad catchall"
[180969.761] (**) Wacom Intuos PT S Finger pad: Applying InputClass "touchpad catchall"
[180969.761] (**) Wacom Intuos PT S Finger pad: Applying InputClass "Default clickpad buttons"
[180969.761] (**) Wacom Intuos PT S Finger pad: Applying InputClass "Wacom class"
[180969.761] (II) Using input driver 'wacom' for 'Wacom Intuos PT S Finger pad'
[180969.761] (**) Wacom Intuos PT S Finger pad: always reports core events
[180969.761] (**) Option "Device" "/dev/input/event16"
[180969.761] (**) Option "Type" "pad"
[180969.772] (**) Option "config_info" "udev:/sys/devices/pci0000:00/0000:00:1a.0/usb1/1-1/1-1.2/1-1.2:1.1/input/input46/event16"
[180969.772] (II) XINPUT: Adding extended input device "Wacom Intuos PT S Finger pad" (type: PAD, id 15)
[180969.772] (**) Wacom Intuos PT S Finger pad: (accel) keeping acceleration scheme 1
[180969.772] (**) Wacom Intuos PT S Finger pad: (accel) acceleration profile 0
[180969.772] (**) Wacom Intuos PT S Finger pad: (accel) acceleration factor: 2.000
[180969.772] (**) Wacom Intuos PT S Finger pad: (accel) acceleration threshold: 4
[180969.773] (II) config/udev: Adding input device Wacom Intuos PT S Pen (/dev/input/event15)
[180969.773] (**) Wacom Intuos PT S Pen: Applying InputClass "evdev tablet catchall"
[180969.773] (**) Wacom Intuos PT S Pen: Applying InputClass "Wacom class"
[180969.773] (II) Using input driver 'wacom' for 'Wacom Intuos PT S Pen'
[180969.773] (**) Wacom Intuos PT S Pen: always reports core events
[180969.773] (**) Option "Device" "/dev/input/event15"
[180969.773] (II) Wacom Intuos PT S Pen: type not specified, assuming 'stylus'.
[180969.773] (II) Wacom Intuos PT S Pen: other types will be automatically added.
[180969.773] (--) Wacom Intuos PT S Pen stylus: using pressure threshold of 27 for button 1
[180969.773] (--) Wacom Intuos PT S Pen stylus: maxX=15200 maxY=9500 maxZ=1023 resX=100000 resY=100000 tilt=enabled
[180969.773] (II) Wacom Intuos PT S Pen stylus: hotplugging dependent devices.
[180969.773] (EE) Wacom Intuos PT S Pen stylus: Invalid type 'cursor' for this device.
[180969.773] (EE) Wacom Intuos PT S Pen stylus: Invalid type 'touch' for this device.
[180969.773] (EE) Wacom Intuos PT S Pen stylus: Invalid type 'pad' for this device.
[180969.773] (II) Wacom Intuos PT S Pen stylus: hotplugging completed.
(EE) 
(EE) Backtrace:
(EE) 0: /usr/bin/X (xorg_backtrace+0x56) [0x7fb0bba1ce96]
(EE) 1: /usr/bin/X (0x7fb0bb866000+0x1bb099) [0x7fb0bba21099]
(EE) 2: /lib/x86_64-linux-gnu/libc.so.6 (0x7fb0b959a000+0x36eb0) [0x7fb0b95d0eb0]
(EE) 3: /usr/lib/xorg/modules/input/wacom_drv.so (0x7fb0afed0000+0x103c0) [0x7fb0afee03c0]
(EE) 4: /usr/lib/xorg/modules/input/wacom_drv.so (0x7fb0afed0000+0xe00d) [0x7fb0afede00d]
(EE) 5: /usr/lib/xorg/modules/input/wacom_drv.so (0x7fb0afed0000+0x62cf) [0x7fb0afed62cf]
(EE) 6: /usr/lib/xorg/modules/input/wacom_drv.so (0x7fb0afed0000+0x650e) [0x7fb0afed650e]
(EE) 7: /usr/bin/X (0x7fb0bb866000+0x95638) [0x7fb0bb8fb638]
(EE) 8: /usr/bin/X (0x7fb0bb866000+0xbfcc9) [0x7fb0bb925cc9]
(EE) 9: /lib/x86_64-linux-gnu/libc.so.6 (0x7fb0b959a000+0x36eb0) [0x7fb0b95d0eb0]
(EE) 10: /lib/x86_64-linux-gnu/libc.so.6 (close+0x2d) [0x7fb0b968679d]
(EE) 11: /usr/bin/X (xf86CloseSerial+0x21) [0x7fb0bb925521]
(EE) 12: /usr/lib/xorg/modules/input/wacom_drv.so (0x7fb0afed0000+0x5c25) [0x7fb0afed5c25]
(EE) 13: /usr/lib/xorg/modules/input/wacom_drv.so (0x7fb0afed0000+0x9c79) [0x7fb0afed9c79]
(EE) 14: /usr/bin/X (0x7fb0bb866000+0xa4948) [0x7fb0bb90a948]
(EE) 15: /usr/bin/X (0x7fb0bb866000+0xbb5b9) [0x7fb0bb9215b9]
(EE) 16: /usr/bin/X (0x7fb0bb866000+0xbb8f8) [0x7fb0bb9218f8]
(EE) 17: /usr/bin/X (WakeupHandler+0x6b) [0x7fb0bb8c1f1b]
(EE) 18: /usr/bin/X (WaitForSomething+0x1c7) [0x7fb0bba1a247]
(EE) 19: /usr/bin/X (0x7fb0bb866000+0x56fe1) [0x7fb0bb8bcfe1]
(EE) 20: /usr/bin/X (0x7fb0bb866000+0x5b3d6) [0x7fb0bb8c13d6]
(EE) 21: /lib/x86_64-linux-gnu/libc.so.6 (__libc_start_main+0xf5) [0x7fb0b95bbec5]
(EE) 22: /usr/bin/X (0x7fb0bb866000+0x4576e) [0x7fb0bb8ab76e]
(EE) 
(EE) Segmentation fault at address 0xa9a8
(EE) 
Fatal server error:
(EE) Caught signal 11 (Segmentation fault). Server aborting
(EE) 
(EE) 
Please consult the The X.Org Foundation support 
at http://wiki.x.org
for help. 
(EE) Please also check the log file at "/var/log/Xorg.0.log" for additional information.
(EE) 
(II) AIGLX: Suspending AIGLX clients for VT switch
(EE) Server terminated with error (1). Closing log file.
```