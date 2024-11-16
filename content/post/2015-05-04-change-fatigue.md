---
author: Jesse Morgan
categories:
- Uncategorized
date: "2015-05-04T21:37:47Z"
guid: http://morgajel.net/?p=1670
id: 1670
title: Change Fatigue
url: /2015/05/04/1670
---

It’s not that I fear change, it’s that I’m weary of it.

This is a good example. I was recently notified that a new version of my operating system (Kubuntu) was release.

After upgrading, here is the list of things that needed to be fixed, in the order that I found them:

- grub timeout needs to be reset to 1 second
- log in and see all of my settings, widgets, backgrounds, etc are gone.
- Rage because the text in konsole is almost illegible
- Go to control panel 
    - Go to look and feel 
        - change desktop theme back to Oxygen, because I \*think\* that’s what I had. Still doesn’t look right
        - Switch cursor theme to Oxygen white. Looks better, but not sure if it’s the same one. I notice that this doesn’t affect the cursor when it’s over the desktop, just over the system settings box.
        - Switch splash screen to none because the new one is Ugly and I see no other options.
    - Go to Colors 
        - try various schemes, none look right
        - try “get new schemes” to find it doesn’t work
        - go with Oxygen because it might be the one I had
    - Go to Fonts 
        - switch all the damn fonts back to ubuntu (I recall this being the previou default)
    - Go to Icons 
        - Switch the theme back to Oxygen, which is what I think I had
    - Rage because my cursor keeps getting bumped and moved because of the touchpad sensitivity being changed
    - Skip down to Input Devices 
        - click on touchpad 
            - go to scrolling 
                - disable edge scrolling
                - disable twofinger scrolling
            - go to sensitivity 
                - enable palm detection
                - make random decreases to width and pressure settings because you can’t tell if that fixed the problem
            - Go to Taps, set all corners to no action, disable tap and drag gestures because I don’t think that’s there.
    - Go Desktop Behavior 
        - go to desktop effects 
            - disable zoom, background contrast, blur, fade, login, logout, maximize, screen edge, sliding popups, translucency, minimize animation, slide, desktop grid, and present windows because they’re all annoying.
- I cannot find “change desktop image” under System Settings despite having clicked on numerous things labeled “desktop” and “workspace”
- right click on desktop to go to desktop settings 
    - notice that all of the preloaded images have generic preview icons.
    - click through and apply each one trying to find my wallpaper I set 26 months ago.
    - Try to open and find the damn thing in my directory
    - find the Open Image dialog doesn’t behave \*at all\*. sidebar shortcuts do not open to the right places, it’s slow, laggy, misaligned and a general clusterfuck.
- Find and edit /etc/default/grub and switch timeout to 1 second, then run “update-grub”

Hope that this is a weird fever dream and reboot.

After a second reboot, it’s slightly more responsive.

- re-add panel on second monitor
- re-add task manager to second panel
- re-enable “only show tasks from current screen”
- try changing wallpaper again, previews still broken, dialog broken.
- Open Konsole and set the shell profile font back to droid sans mono 10
- Attempt to start virtualbox, get error that drivers need to be rebuilt

I’ve to re-add my desktop widgets (memory usage, cpu, weather, etc), but they appear to be gone now.

- get error that headers are missing
- switch back to classic application K menu

Overall it was disappointing and frustrating, and I wish I hadn’t. While I understand it’s mainly due to the new version of plasma, it simply wasn’t ready for release. I need to get stuff done, and I can’t wait for this to be fixed.

Update: Since writing this, I’ve decided to re-image my machine using mint with KDE. So far, it’s much better than 15.04.