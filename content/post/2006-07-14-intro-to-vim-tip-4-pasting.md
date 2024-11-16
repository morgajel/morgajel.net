---
author: Jesse Morgan
categories:
- Linux
- Utility
- Vim
date: "2006-07-14T07:56:42Z"
guid: http://morgajel.net/2006/07/14/145/
id: 145
title: 'Intro to Vim Tip #4 (Pasting)'
url: /2006/07/14/145
views:
- "66"
---

If you need to paste into vim from somewhere else, and your code has tabs or spaces in it, you’ll notice that vim may add extra tabs. see, vim doesn’t see it as a paste event, it sees it as “you typing really fast”- and one thing vim does will is auto-indent. The problem is when you paste, you don’t want auto-indentation because your code is already indented.

to temporarily turn off auto-indenting, try this from insert mode:

```
[esc]:set paste[enter]
```

go back to insert mode and you should be able to paste without the extra tabs.