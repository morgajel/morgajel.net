---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-09-10T12:04:07Z"
guid: http://morgajel.net/?p=1441
id: 1441
title: JVM Thread CPU details using PS and LWP
url: /2013/09/10/1441
---

First, capture a threaddump from your process (jvisualvm, kill -3 or jstack should do the trick).

At the same time, get a list of the LWP threads from PS for your process

```
ps -eLo pid,lwp,nlwp,ruser,pcpu,stime,etime,args -p {PID} >lwpthread.txt
```

Once you have it, sort by CPU utilization and tail the list to get the worst four offenders:

```
cat lwpthread.txt |sort -n -k 5,5 |tail -n 4 |awk '{print $2}' |xargs
```

Now take the resulting list and throw it in a loop and convert them to hex (note, it must be lowercase!):

```
{ for i in 1882 4709 19306 20088 ; do echo "obase=16; $i"|bc | awk '{print tolower($0)}'; done ; } |xargs
```

which produces our nids, which we can grep out of the thread dump:

```
grep '75a\|1265\|4b6a\|4e78' threads2.txt  -A 4
```

and will show you the top 4 threads that are chomping away at your CPU.