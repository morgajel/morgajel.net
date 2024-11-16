---
author: Jesse Morgan
categories:
- Electronics
- Linux
- Open Source
- Rant
date: "2006-02-26T10:30:25Z"
guid: http://morgajel.net/?p=96
id: 96
title: Kiss my Ass, Linksys (pt. 2)
url: /2006/02/26/96
views:
- "48"
---

I’ve apparently become either very brave, or very stupid in my old age. Last night at 10pm I reset my router and flashed it’s firmware with an open source alternative.

A few years back, some Linksys tech realized the usefulness of some open source components and implemented them in the firmware of their routers- they just forgot to follow the license agreement of the software they used and didn’t tell anyone or release the source code and the changes they made. Along comes some hacker and recognizes the code, and a shitstorm of controversy hit Linksys, which ended up with them agreeing to release their changes (as they should).

From this, a lot of open source projects popped up with re-implementations of the firmware. One of them was [DD-WRT](http://wrt-wiki.bsr-clan.de/index.php?title=Main_Page), which added a lot of new functionality to the linksys router. This is what I’m using now.

Among the many new features, I have access to the following: WPA2, pptp service on the router, syslog, MRTG, more than 10 port groupings that can forward, and SSH access. By the way- this is all running on a linux kernel, so the regular linux stuff applies.

Everything was running smoothly by 2am- would have had it done by midnight, had I been using the proper password. But that’s over and done with. Oh, remember how I got pissed at linksys for their visually impaired-unfriendly text interface?

![](http://morgajel.com/screenshots/dd-wrt.png)

Works fine now, so I don’t want to hear the bullshit that they couldn’t do it. Here’s what the new interface looks like:

![](http://morgajel.com/screenshots/dd-wrt1.png)

A bit prettier, if you’ve ever seen the original linksys interface. So it’s prettier AND more functional.

The sad part here is I’d consider buying another linksys router for no other reason than to run dd-wrt. If it holds up with no problems for a month, I’m gonna be sending the author some “good job, keep it up” money. I’m impressed, so far.