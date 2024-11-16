---
author: Jesse Morgan
categories:
- FreeBSD
- IRC
- Linux
- Open Source
- Rant
date: "2006-05-03T10:44:25Z"
guid: http://morgajel.net/2006/05/03/124/
id: 124
title: Dependencies of Dependencies in FreeBSD
url: /2006/05/03/124
views:
- "198"
---

Something that is really aggrivating the hell out of me is FreeBSD’s package management system. I’ve heard people go on and on about how it’s the best out there, but frankly I’m unimpressed.  
The main reason for this is there is no way to determine ALL of the dependencies that are going to be installed when I install a package.  
Lets do a comparison of a freeciv install on my workstations vs the freebsd server:

```
genbox ~ # emerge -tp freeciv

These are the packages that I would merge, in reverse order:

Calculating dependencies ...done!
[ebuild  N    ] games-strategy/freeciv-2.0.8
[ebuild  N    ]  media-libs/sdl-mixer-1.2.6-r1
[ebuild  N    ]   media-libs/smpeg-0.4.4-r7

```

As you can see, freeciv is dependent on sdl-mixer- not only that, sdl-mixer is dependent on smpeg, meaning for freeciv to be installed, BOTH sdl-mixer and smpeg need to be installed beforehand. you see not only the dependencies, but the dependencies of the dependencies. Fortuately for this example, there were only 2 layers of dependencies and they were fairly small.

Now, lets look at freebsd- to see the dependencies, you can’t actually do it from the command line- you have to visit their website: http://www.freebsd.org/cgi/ports.cgi?query=freeciv&amp;stype=all&amp;sektion=all  
We can see there are a couple of matching packages, but we’re only interested in

```

freeciv-2.0.8 
A civilisation clone for X11; multiplayer
 Long description : Sources : Changes : Download
 Maintained by: infofarmer@gmail.com
Requires: Xaw3d-1.5E_1, expat-2.0.0_1, fontconfig-2.3.2_4,1, 
freetype2-2.1.10_3, gettext-0.14.5_2, jpeg-6b_4, 
libdrm-2.0.1_1, libiconv-1.9.2_2, pkgconfig-0.20_2, png-1.2.8_3, 
python-2.4.3, tiff-3.8.2, xorg-libraries-6.9.0
```

This shows all dependencies, not just the ones that need to be installed- but only one layer. What packages will freetype require installed? how about xorg-libraries? The best answer I’ve been able to find as of yet is “look them up for each dependency” and then for their dependencies, and then theirs. All of this manually, all of this through their website. plus, you have to make sure that each package you look up isn’t already installed. How many packages will it take?

I have yet to find a way to show dependencies of dependencies in freebsd, and several very competent BSD admins I know haven’t been able to answer this question.

My question is, WHY!?!? This is entry level base functionality that debian, gentoo, mandriva, suse and even freaking RHEL can do- why can’t BSD, the people behind the ports system, do this?

I’m afraid I know the answer to this question- they’re comfortable with what they have. I truly hope I am wrong. If anyone knows of the proper FreeBSD way to find dependencies of dependencies on the command line, please, let me know.