---
author: Jesse Morgan
categories:
- Uncategorized
date: "2009-06-22T12:31:36Z"
guid: http://morgajel.net/?p=513
id: 513
title: Jaunty&#8217;s Accidental Second Chance
url: /2009/06/22/513
views:
- "48"
---

So last week in an attempt to update… something (\*shrug\*, don’t remember what), I upgraded to intrepid, which bumped me up to kde4. It was a lot smoother this time around, but there were a few bugs left… mainly the fglrx driver didn’t like me. so I decided to go full bore and try jaunty again. it’d been 2 months since initial release, so it should be a bit more stable.

Well, things sorta exploded. fglrx still wouldn’t work. I ended up burning a copy and doing a clean install (my home dir is on another partition now so no data loss!) I got jaunty up and running fine with the radeon driver rather than the fglrx driver, and started getting everything into place. And by ‘use the radeon driver’ I mean remove all fglrx packages so it defaults to radeon (or whatever else).

Knetworkmanager still sucks, but the plasmoid worked properly this time and is quite nice.

When I got to work I burned half an hour trying to get dual monitors working (again, trying to get fglrx working) and found xrandr works great now. I used the following command to set up my dual monitors:

> sudo xrandr –output LVDS –preferred –mode 1600×1200 –auto –left-of DVI-0 –output DVI-0 –auto –mode 1680×1050

and the following is the ONLY xorg config I have:

> Section “Device”  
>  Identifier “Configured Video Device”  
> EndSection
> 
> Section “Monitor”  
>  Identifier “Configured Monitor”  
> EndSection
> 
> Section “Screen”  
>  Identifier “Default Screen”  
>  Monitor “Configured Monitor”  
>  Device “Configured Video Device”  
>  SubSection “Display”  
>  Virtual 3280 1200  
>  EndSubSection  
> EndSection

Note the only thing I added with the Display subsection. Anyone who’s been using Linux on GUIs for the past 10 years can tell you how amazing that configuration is, and how far Xorg, xrandr and linux as a whole have come.

Amarok gave me some problems, but I was just missing some codecs. I forced KDE to 96 DPI and the screen looks great. The one thing I’m missing now is a functional Quickaccess plasmoid (on that does vertical mode properly) and I’m good. maybe I’ll write about the plasmoids (widgets) I’m using later.

There are the occasional video glitches here and there- mainly flickers or lines that flash for a 10th of a second and then are gone for an hour or 3. The other thing is when I pop open krunner and begin to type, there’s a little noise in the box. over my text as it tries to resize the window.

Other that that minor annoyance I think I’ve been converted to kde 4. That’s an amazing turnaround.