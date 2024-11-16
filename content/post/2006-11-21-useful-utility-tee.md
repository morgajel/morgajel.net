---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Reviews
- Utility
date: "2006-11-21T10:30:42Z"
guid: http://morgajel.net/2006/11/11/55/
id: 55
title: 'Useful Utility: tee'
url: /2006/11/21/55
views:
- "70"
---

I have two requirements for a program being on this list: the first one is it has to be a utility- something scriptable or usable on the command line. The second is it also needs to have multiple arcane flags that I can write about, or just be so unknown that it’ll bring it to the attention of people that have never heard of it. Tee falls into the “never heard of it” group. It may only have two flags, but it’s useful nonetheless.

Tee splits a STDIN stream in two, sending one stream out output to a file, and the other to STDOUT. The data is identical; this just allows you to take a snapshot of what the data is like at a certain point a long piped command. For example, lets suppose you had a nice pretty command like this:

```

ls -lrt |awk '{print $6" "$7" "$8" "$3"  "$9}'|grep morgajel|tail -n 3
```

This provides you with the date, owner and filename of the 3 newest files in the current directory. Suppose you wanted a list for all the users as well as just morgajel’s latest three

```

ls -lrt |awk '{print $6" "$7" "$8" "$3"  "$9}'|tee all.txt|grep morgajel|tail -n 3
```

By adding the “tee all.txt” a copy of the stream at that point is diverted into a text file. you can view that later. The example is a bit fake, but you’ll eventually run into something needing this functionality. When you do, you can use tee. The only two real flags of interest are -a which appends data to the file rather than recreating it, and -i to ignore interrupts, but that could be really bad if you need to ctrl-c out of something…

either way, enjoy.