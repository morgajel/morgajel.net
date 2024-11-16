---
author: Jesse Morgan
categories:
- Linux
- Work
date: "2006-07-24T13:35:31Z"
guid: http://morgajel.net/2006/07/24/149/
id: 149
title: Gentoo is too powerful
url: /2006/07/24/149
views:
- "42"
---

So I learned something new today. Apparently our UPS is rated to 3K, and we just added 3 new machines to it, putting it at 4 out of 5 bars for load.

These three new machines are running gentoo on dual-xeon processors. I made the mistake of attempting to recompile subversion on all three machines at the same time.

Suddenly, all the servers stopped responding. When we went into the serverroom, all the machines in the dell rack were off- the UPS had tripped. Apparently the combined churning of 6 hyperthreaded 3ghz xeons was enough to overload the UPS and trip it. Awesome.

BEHOLD THE POWER OF GENTOO!

I’ll make sure not to do that again… or more likely, I’ll tell management we need a new/another UPS for that rack. It was a good stresstest- we know know that if those servers have any load put on them, they’ll take down all of our stuff (in that rack).