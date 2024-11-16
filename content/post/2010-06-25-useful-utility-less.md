---
author: Jesse Morgan
categories:
- Open Source
- Utility
date: "2010-06-25T10:00:29Z"
guid: http://morgajel.net/?p=64
id: 64
title: 'Useful Utility: less'
url: /2010/06/25/64
views:
- "92"
---

> Less is more.

That’s the common joke about less- It provides the same functionality as the older utility, more; but oh, how much more than more!

Less allows you to easily scan backwards as well as forwards- something more is not too good at (though it is possible). Less also allows you to navigate with arrow keys, page up and page down, home and end.

Less provides a quick way to view files as well- Many editors (like vim) need to read an entire file first, and often create a temporary copy (for editing) of the file. This isn’t a problem until you try to browse a 56 gig log file of a rabid JBoss instance- if your root directory isn’t large enough, chaos ensues. Less circumvents this by only loading a page at a time. Yes- searches do take longer, but it beats crashing a system.

### Useful Commands:

- **R**: Repaints the screen, rereading the file from the disk. useful if the file is constantly changing (like a logfile)
- **/ and ?**: Just like in vim, used for searching forwards and backwards
- **g and G**: Jumps to the top or bottom of the file, just like vim
- **:n and :p**: Jump to the next/previous file (if you pass less more than one file)
- **q**: quits. slightly important
- **! \[cmd\]**: runs a given command outside of less- useful if you can’t remember the output of another command and need a quick reminder (ifconfig, tail, route, ls, etc)

### Useful Flags:

- -N: Shows line numbers
- -s: squeeze adjacent blank lines into one
- -R: shows raw control characters, a.k.a. “turns color on!”

As usual, there are dozens of more options, flags, variables and behavior- you’d be surprised what less can do for you.