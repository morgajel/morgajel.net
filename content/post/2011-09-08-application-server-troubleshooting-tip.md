---
author: Jesse Morgan
categories:
- Work
date: "2011-09-08T20:40:21Z"
guid: http://morgajel.net/?p=1104
id: 1104
title: Application Server Troubleshooting tip
url: /2011/09/08/1104
views:
- "606"
---

We recently ran across a problem in production that we could not replicate in lower environments. Since this is not only a high use application, but an exceptionally “chatty” app, searching the logs was an excersise in futility (\*one\* of yesterday’s production logs was 6,975,291 lines long, with multiple logfiles per app, multiple apps and multiple servers).

So how do you find a needle in the haystack? Get a smaller haystack. In the quickest window possible, perform the following three steps

1. tail log1 log2 log3 log4 &gt;combined.log
2. reproduce error
3. ctrl-c tail process as quickly as possible

Doing so reduced our 11,480,799 lines (with 780,527 errors) to 1200 lines.