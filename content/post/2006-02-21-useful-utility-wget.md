---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Reviews
- Utility
date: "2006-02-21T09:35:58Z"
guid: http://morgajel.net/?p=53
id: 53
title: 'Useful Utility: wget'
url: /2006/02/21/53
views:
- "58"
---

Wget is useful for a lot of things- downloading images from a directory listing, mirroring a website, recursively fetching one subdirectory of a website, etc. The main focus as you can tell is downloading from the web(http, https, ftp) in a non-interactive manner.

There are a lot of flags to change the behavior, and you can get all sorts of wild behavior by mixing and matching those flags. The most straightforward use is this:

```

wget http://example.com/software_pkg.tar.gz
```

This is the best tool for grabbing packages off a website to install on a headless machine. I can’t count the number of times I’ve needed to install something on my server manually and used wget to do it.

###  Useful Flags

| Flag | Use |
|---|---|
| -nc | no clobber. useful for re-downloading sites, but not files you already have. |
| -N | similar to -nc, except it overwrites if the file on the server is newer than the local version. useful for keeping mirrors up to date. |
| -r | work recursively through a site and grab all the files |
| –spider | doesn’t download the files, just checks to see if they’re there. |
| -l x | designates to recursively download to a “depth” of x |
| -m | mirrors a site. turns on timestamping, infinite recursion and keeps ftp indexes. |
| -X list | exclude a list of directories from download. useful for preventing yourself from mirrors 30 gigs of video files |
| -np | no parent. prevents you from accidentally going up into the parent directory when downloading recursively. |

That’s a good start. There are a lot more options, but those listed above (and combinations of them) are probably enough to get you moving. Use this tool. Play with it. Mirror a site, play with the -N and -nc flags. Please feel free to include your own wget recipes below in the comments.

K\_F, dev\_null, I’m looking at you.