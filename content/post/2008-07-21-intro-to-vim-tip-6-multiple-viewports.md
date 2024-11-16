---
author: Jesse Morgan
categories:
- Open Source
- Vim
date: "2008-07-21T16:02:08Z"
guid: http://morgajel.net/?p=261
id: 261
title: 'Intro to Vim Tip #6 (Multiple viewports)'
url: /2008/07/21/261
views:
- "74"
---

One feature of vim I don’t use enough is the ability to split the screen and view multiple files at once. I use this feature all the time when I use vimdiff, but not really any other time. I thought I’d take a moment to lay out some uses of it (thank [Linux.com](http://www.linux.com/articles/54157) for the reference):

From Command Mode:

- :sp splits the screen horizontally
- :vsp splits the screen vertically
- Ctrl-w Ctrl-w moves between viewports
- Ctrl-w \[right arrow\] moves active viewport 1 to the right
- Ctrl-w \[left arrow\] moves active viewport 1 to the left
- Ctrl-w 3\[left arrow\] moves active viewport 3 to the left
- Ctrl-w q will close the active window.

and remember that you can open a file with 😮 filename in the newly created viewport