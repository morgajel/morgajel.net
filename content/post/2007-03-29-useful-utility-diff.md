---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Utility
date: "2007-03-29T09:55:16Z"
guid: http://morgajel.net/2006/02/21/85/
id: 85
title: 'Useful Utility: diff'
url: /2007/03/29/85
views:
- "55"
---

Diff is a handy little command used to compare two text files- useful if trying to determine what’s changed in different versions of files, used by subversion to show what files have been changed, and can even create patch files for updating sourcecode. So what are some of the more useful flags?

\* -i lets us ignore any capitalization changes  
\* -b lets us ignore any spacing changes  
\* -B ignore blank lines  
\* -w just ignore all white spaces  
\* -q just say if the files are different  
\* -y side by side comparison  
\* -r recursively compare directories  
\* -d find a smaller set of changes  
\* -u unified format

I often use the unified format(-u) simply because I find the +/- more intuitive than &gt;/file.patch.

If you have any other uses for diff, leave them in the comments below.