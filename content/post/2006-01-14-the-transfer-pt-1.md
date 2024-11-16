---
author: Jesse Morgan
categories:
- Linux
date: "2006-01-14T14:20:14Z"
guid: http://morgajel.net/2006/01/14/42/
id: 42
title: The Transfer
url: /2006/01/14/42
views:
- "45"
---

So I’m running out of space. on everything.

my workstation has a 40 gig split in half with 1/2 linux and half a long dead windows partition (I got this drive in 2001 or thereabouts.)  
the windows partition has been repurposed, but it’s still a pain in the ass. since the windows partition appears first on the drive, there was no way to wipe it and merge it with the other parition (easily). The problem I was having was large software installs (Neverwinter Nights, World of Warcraft) required a LOT of space- 5 gigs each. It adds up quickly.

Jackie’s machine is in the same situation- not barely enough room for neverwinter nights.

When I took the job at CSX, I needed a redhat install to tinker with- so I went out and purchased a 200gig and put it in my workstation.

My webserver has a 10 gig drive, which has been holding for a good 2 or 3 years. Well, the webserver had a bit of an accident a week or 3 ago and it’s HD filled up. so I’m left in a bit of a pickle.

**THE SOLUTION**

So here’s what I did. I moved all the contents of the 40gig in my workstation to the 200 gig. took a bit of tinkering, but it’s done. it should be ok for a good long while.

I’m going to put my 40 gig in jackie’s machine, and transfer everything of hers onto my HD with a single partition. She has windows at work, so she really doesn’t need the partition anymore- I don’t think she’s used it in a long time anyways.

finally, I’m going to place jackie’s 40 gig in the webserver and transfer the contents of the 10 gig to the 40 gig.

I’ll then shelf the 10 gig.

sounds simple, huh?

**Update: 1:00 pm**  
So I got my workstation taken care of- now I just need to transfer jaxon’s stuff around- I’ll wait for jackie to get back before attempting that though.

**Update: 9:00 pm**  
Jackie’s workstation, jaxon, as transitioned smoothly. It’s now running on my hard drive and she has 20 gigs of free space.  
P-nut the webserver is next. While P-nut is down, morgajel.net, morgajel.com and myjaxon.com will be down as well, as well as the audio stream and IRC.

**Update: 12:45 am**  
P-nut is up and running. I switched him over to grub (rather than lilo), set up a boot partition and increased his swap space.  
it is gratifying to see this:

```

Filesystem            Size  Used Avail Use% Mounted on
/dev/hda3              36G  6.3G   28G  19% /
/dev/hda1             183M   16M  157M  10% /boot
```

before it was at 6.3G out of 9.2G, and that was AFTER I cleaned.

So I think my work on this project is done. I’d like to figure out why DNS is still not working properly, but that can wait for tomorrow.