---
author: Jesse Morgan
categories:
- FreeBSD
- Open Source
- Rant
date: "2007-03-24T14:50:47Z"
guid: http://morgajel.net/2007/03/24/116/
id: 116
title: What makes freeBSD feel old?
url: /2007/03/24/116
views:
- "80"
---

 This is a list of all the things that make it feel old. I started this while working at a place that ran a lot of FreeBSD machines. I never got around to finishing it because we started implementing linux boxes, but I think the complaints are still valid. The real shame is that I only wrote down 6 out of about 100 different things. Mostly it’s trivial stuff, but trivial stuff should be the easiest to fix- the FreeBSD people had a real fear of painting barns (take that as you will).

- No Color during the install. none at all. even getting syntax highlighting in vim was a pain. Perhaps I’m just recalling the FreeBSD 3.1 installations they had, but it seemed a mess.
- The ASCII art in the install menu. It gives the feel of a 1988 BBS. It didn’t help that there was a keyboard driver bug in 6.0 or 6.1 that prevented me from actually selecting anything at the menu.
- Speaking of the install, I think out of the 30 or so installs I attempted, only 1 or 2 were even remotely close to trouble-free. I tried the easy, medium and advanced installs- perhaps it was just the box I was installing, or maybe bsd just hates me for calling it names.
- The partitioning tool makes [cfdisk look futuristic](http://morgajel.com/screenshots/cfdisk.png). Does FreeBSD actually listing my 200 Gig hd by block ranges? Let me just say, that’s WAY better than giving info in a useful unit.
- network cards all have different device names- it seems a mess to have 4 Nics and have bge0, sf0, anxl0, and an0. It’s a bad example, but I guess it’s because I prefer the uniformity of eth0, eth1, eth2 in linux (which can be arranged by setting parameters in /etc/modprobe.d/options).
- rc.conf always seemed to be the most cryptic thing ever- not that it was hard to read, you just never knew what you could put in there. Whenever I asked around for any way to tell what options were available for a given package to add them into rc.conf, the answer always turned to “well download the source and check.” I’m not sure if that was \[several\] someone’s idea of a joke, but it really made me dislike it. compare that to emerge -v package in gentoo, which lists all of the compile option in an easy to read format.