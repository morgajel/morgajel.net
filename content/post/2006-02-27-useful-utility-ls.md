---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Reviews
- Utility
date: "2006-02-27T11:01:59Z"
guid: http://morgajel.net/?p=54
id: 54
title: 'Useful Utility: ls'
url: /2006/02/27/54
views:
- "58"
---

With the exception of maybe cd (which is boring), ls is probably the command you’ll use the most if you do a fair amount of work at the command line.

ls lists files. It’s simple enough concept, but there’s a lot of information about those files that you can list as well. ls by itself will list the contents of all regular files and directories in the current directory. you can provide with with a target such as ls /foo or with multiple targets like ls /mnt /opt or ls foo.png foo.jpg foo.gif. You can also feed it command line special characters like ls foo.\* or document\[0-9\].txt.

ls -l is probably the most useful and used flag you’ll use, since it lists permissions, ownership, timestamp, size in bytes and some other information. You can manipulate this enrty further by adding the -r and -t flags, which orders the list by timestamp, and then reverses it. This makes the bottom of the list show the most recently modified file- very useful when dealing with logs.

ls foo/ will list the contents of foo/, but if you add a -d flag, it will simply list the properties of foo/ itself. ls -a foo/ will list not only the contents of foo/, but the hidden files, directories and the special ./ (which represents foo/ ) and ../ (which represents the parent directory). ls -A will list all files EXCEPT the .. and ../ directories.

ls -s will list the file sizes next to the file name, while ls -sh will show the file sizes in a human readable format, such as k, m or g. If you want to know if a file is executable, symlink, directory or pipe, you can try ls -F, which is much shorter than ls –color, which colorizes files according to their type. The last really useful item you might be interested in is ls -R which shows the contents recursively of every subdirectory below the target directory.

As always, I’m just scratching the surface. feel free to include your own uses and flags in the comments below.