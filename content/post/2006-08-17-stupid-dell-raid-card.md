---
author: Jesse Morgan
categories:
- Linux
- Work
date: "2006-08-17T11:55:38Z"
guid: http://morgajel.net/2006/08/17/157/
id: 157
title: stupid dell raid card..
url: /2006/08/17/157
views:
- "251"
---

so the devs are having a hell of a time with the new dev server- it’s constantly locking up on certain threads for 30-45 seconds. apache mainly, but occasionally grep and other things. It appears to be completely random. I’ve been pulling my hair out trying to find the problem, and I think I got it nailed down thanks to a post on the gentoo forums (<http://forums.gentoo.org/viewtopic-t-189180-highlight-writethru.html>) that said to change some settings in the raid card’s firmware… I tested this on the prod machine since it’s not in use yet(they’re both dell 2850’s with the lsi megaraid cards) and benchmarked before and after with bonnie++… here are the results(- is original, + is new):

```

 Version 1.93c       ------Sequential Output------ --Sequential Input- --Random-
 Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
 Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
-prod             7G   331  98 27139  10 17554   4  1156  99 65858   7 371.8   5
-Latency               104ms   41538ms     804ms   19824us   46554us     631ms
+prod             7G   335  98 54977  23 27923   7  1075  99 61596   6 342.9   4
+Latency             76755us     331ms     590ms   12104us   24805us     414ms
```

Notice the second column under Latency? 41538ms vs 331ms – a 40 second latency when using the dell default settings…

I think this is where my lag issue is coming from… here’s hoping.  
I’ll report back when I make the change on dell and see if the devs are still having lag problems.

Update-  
so I switched the write policy to ‘write-thru’ instead of write back and changed the readpolicy to normal rather than adaptive… changed the benchbark, but not the problematic behavior- well, actually now, ALL of the devs are locking up at the same time…