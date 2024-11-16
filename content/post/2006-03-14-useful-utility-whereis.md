---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Reviews
- Utility
date: "2006-03-14T12:00:40Z"
guid: http://morgajel.net/2006/03/14/92/
id: 92
title: 'Useful Utility: whereis'
url: /2006/03/14/92
views:
- "40"
---

Whereis is an older utility- it’s functionality shows us of a time when a program not only had a man page, but also stored the source on the machine in question. That’s becoming more rare as programs like firefox come into play- firefox, for example, has no man page and doesn’t install the source (due to size issues).

Whereis locates the binary, man page and source of a given command on the current machine.

```

 $ whereis ls
ls: /bin/ls /usr/bin/ls /usr/X11R6/bin/ls /usr/bin/X11/ls 
/usr/share/man/man1/ls.1.gz /usr/share/man/man1p/ls.1p.gz

 $ ls -l /bin/ls /usr/bin/ls \
/usr/X11R6/bin/ls /usr/bin/X11/ls /usr/share/man/man1/ls.1.gz  \
/usr/share/man/man1p/ls.1p.gz
-rwxr-xr-x  1 root root 70708 Jan 24 15:53 /bin/ls
lrwxrwxrwx  1 root root     7 Jan 24 15:53 /usr/X11R6/bin/ls -> /bin/ls
lrwxrwxrwx  1 root root     7 Jan 24 15:53 /usr/bin/X11/ls -> /bin/ls
lrwxrwxrwx  1 root root     7 Jan 24 15:53 /usr/bin/ls -> /bin/ls
-rw-r--r--  1 root root  6403 Mar  6 10:52 /usr/share/man/man1/ls.1.gz
-rw-r--r--  1 root root  8070 Mar  6 10:52 /usr/share/man/man1p/ls.1p.gz
```

The flags are a little boring- you can limit your searches to binaries (-b), source (-s) or manuals (-m).

Something I found interesting is the capitalized versions of those flags states to only check the directories immediately listed after, but only when the last directory is followed by a -f flag.

```

$ whereis -m -M /usr/share/man/man1/ -f  ls
ls: /usr/share/man/man1//ls.1.gz
```

I’ve never seen anything like this before- a space-delimited option with a flag terminator. I’m sure other examples exist, I just can’t think of any. Normally you’d just use commas as seperators and follow it with a whitespace to terminate. The man page also states that due to an specific function it uses(chdir), make sure you use full, not relative paths.

Honestly I was hoping whereis would have a little more meat to it. This is probably one of the more useless programs I’ve written about.