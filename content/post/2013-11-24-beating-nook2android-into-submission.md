---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-11-24T19:53:41Z"
guid: http://morgajel.net/?p=1467
id: 1467
title: Beating Nook2Android into Submission
url: /2013/11/24/1467
---

Jackie has been asking for a while to convert her nook color (bnrv200) to android, so I finally decided to take it on (since I was on vacation).

Rather than muddle through piecing together details from 100 sources- I took the “easy” route and used [nook2android](https://www.n2acards.com). Since I wanted to do this now rather than in a few days, I bought the installer rather than a pre-installed card. Should be simple, right?

**UNFORTUNATELY…**

I didn’t see the part where they said “Windows only” for the installer- you see, rather than provide a simple ISO image, they provided an EXE.

Luckily I have a windows VM available. I then spend the next two hours trying to get virtualbox to recognize my SD card as an SD card- it would only show up in the installer if it was an SD card, but virtualbox treated it as a simple harddrive, so while I could run the EXE, the exe couldn’t see my sd card.

After trying and researching half a dozen things, I looked at the temp files that were generated by the installer and noticed one file was a 200meg “.imz” file. I wasn’t 100% sure, but I figured this might be the actual image. I copied this back to linux and threw it on the sdcard with dd- It wouldn’t boot. On a hunch I used unzip on the imz file and managed to fill the 5 gig that remained on my harddrive :/

After deleting and clearing space, I was able to unzip it as an 8 gig ima file. I copied this to the SD card via DD and BOOM, it now boots.

It took quite a bit of trial and error, but if it came up again, I could do it again.