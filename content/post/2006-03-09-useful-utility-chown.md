---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Reviews
- Uncategorized
- Utility
date: "2006-03-09T10:25:01Z"
guid: http://morgajel.net/2006/03/02/101/
id: 101
title: 'Useful Utility: chown'
url: /2006/03/09/101
views:
- "44"
---

Since I covered chmod last week, I figured I should touch upon chown this week. chown is infinitely less complex than chmod because you don’t have to worry about actual permissions. chown is mainly used by root, but I suppose it could be used by others as well, although it will happen much less often.

chown can change the owner and group of a file or files.

Standard usage goes something like this

```

 $ chown morgajel:svngroup samplefile
```

You can check to make sure changes took by using

```

 $ ls -l samplefile
```

Much like chmod, you can use -c to show changes, -v for verbose and -f for quiet. There’s also the -R recursive flag, which works in a similar fashion, but it has several related flags that can be used in conjunction.

One interesting feature of chown is how it handles symbolic links when working recursively- the default behavior is not to traverse and just ignore them. If you use the -H in conjunction with -R, it will traverse the command line argument if it’s a symbolic link. In other words

```

 $ ls -l mywww
  lrwxrwxrwx  1 morgajel users 9 Mar  9 10:08 mywww -> /var/www
  $ chown bob   -R -H ~/mywww
```

will recursively change the files in /var/www, but not symbolic links located IN that location. This behavior differs from -L flag used in conjunction with -R, which will traverse ALL symbolic links encountered.

This can be useful to know if you need to change ownership of an entire directory structure and it’s full of symbolic links to other places.

One interesting flag I found is the conditional –from=user:group flag. It is appended to any other chown command and will only change the file if it meets the condition of having a particular user and/or group. This little tidbit could save you a couple lines of shell scripting down the road- I could see it being useful on rare occasions.

The last flag of interest is the –reference=file flag, where you can reference a file and use it’s user and group to set the target file without explictly stating it. Not incredibly useful, but interesting nonetheless.

Sort of a lame article, I know, but the midi stuff has been keeping me busy.