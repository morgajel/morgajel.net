---
author: Jesse Morgan
categories:
- FreeBSD
- Open Source
- Reviews
- Work
date: "2006-03-22T07:18:57Z"
guid: http://morgajel.net/2006/03/22/115/
id: 115
title: First Impressions
url: /2006/03/22/115
views:
- "33"
---

Holy crap, welcome to 1980. The FreeBSD install is going to take a little bit longer than I thought. I booted off the Install CD and the first thing I noticed was the lack of color. Not shiny pretty GUI color, but angry fruit salad color. As pages of white text on black blackground whizzed past my screen, nothing stuck out as important; I noticed no dividers between sections. This is a very small, trivial thing, but it is nice. Compare and contrast with say a gentoo liveCD- I’m not even talking about the background images and framebuffered text; I just mean colored headers.

Never underestimate the power of syntax coloring.

Next up, HD partitioning- the tool they’re using is sorta like cfdisk, but ugly. I panicked at this point because I wasn’t sure where I wanted to install fbsd, and I was having second thoughts about completely wiping draccus. None of the partitions were labeled in silly ways like “hda1”, just (IIRC) ranges of blocks. Solution? Jumped back to linux, found a 15 gig partition that I had forgotten about, moved the stuff to the 150 gig linux partition, then booted off an unbuntu LiveCD I picked up at the FOSE Convention, and am currently resizing the 150 gig, deleting the 15 gig, and merging the free space so I can dedicate aroung 70 gigs to FreeBSD.

Unfortunately I’ve run out of time this morning to actually install FBSD, but I’ll get to it tonight. GParted is a very nice tool, BTW. Hopefully it’ll be done running e2fsck on the HD by the time I get home 🙂