---
author: Jesse Morgan
categories:
- Open Source
- Reviews
- Utility
date: "2006-02-16T12:54:27Z"
guid: http://morgajel.net/?p=51
id: 51
title: 'Useful Utility: cut'
url: /2006/02/16/51
views:
- "42"
---

This is the first of a series of entries I’d like to do. Each week I’m gonna discuss a simple linux utility that you may or may not be familiar with.

First up is **cut**.

Cut can be used to shape data that is piped to it. for example, lets suppose you wanted a list of the real names and user names from the /etc/passwd file where you actually have a real name. Here’s the data as it’s currently stored.

morgajel:x:1000:100:Jesse Morgan:/home/morgajel:/bin/bash  
cullenp:x:1011:100:Paul Cullen:/home/cullenp:/bin/bash  
clstopher:x:1013:100:Chris Shaffer:/home/clstopher:/bin/bash  
fletchmr:x:1014:100:Matt Fletcher:/home/fletchmr:/bin/bash  
kristianf:x:1034:100:Kristian F.:/home/kristianf:/bin/bash

We can run this through cut, telling cut to use the colon(:) as the delimiter and to only take get the user name and the real name:

cut -d: -f 1,5 /etc/passwd

morgajel:Jesse Morgan  
cullenp:Paul Cullen  
clstopher:Chris Shaffer  
fletchmr:Matt Fletcher  
kristianf:Kristian F.

Suppose you want to just cut characters off an entry? we can do that too. Suppose you want to look at who owns the sites hosted on a machine:

```

morgajel@p-nut ~/ $ ls /var/www/ -l
total 84
drwxrwxr-x  23 morgajel ourselves 4096 Jan 14 10:32 morgajel.net
drwxrwxr-x  27 morgajel ourselves 4096 Jan 28 09:45 morgajel.com
drwxr-xr-x   5 jaxon    apache    4096 Oct  9 11:14 myjaxon.com
drwxr-xr-x   2 morgajel apache    4096 Jan 24  2005 test
```

That shows it, but there’s a lot of cruft that isn’t useful if you’re trying to script the information into something useful.  
We can visually count characters and figure out that the usernames are between the 16th and 24th characters and the site name starts and the 52 character and extends beyond. We can use the -c flag in cut and specify which characters we want to keep:

```

morgajel@p-nut ~/ $ ls /var/www/ -l|cut -c 16-24,52-

morgajel  morgajel.net
morgajel  morgajel.com
jaxon     myjaxon.com
morgajel  test
```

Anyways, that was a quick run-through. There are more powerful tools that can do this an more, such as awk and sed, but this is useful for spur of the moment scripts that you don’t plan on keeping long. Always check the manpage for more details or leave a comment if you have more helpful tips.