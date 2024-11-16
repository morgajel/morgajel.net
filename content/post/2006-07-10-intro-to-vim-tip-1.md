---
author: Jesse Morgan
categories:
- FreeBSD
- Linux
- Open Source
- Utility
- Vim
date: "2006-07-10T08:40:17Z"
guid: http://morgajel.net/2006/07/10/139/
id: 139
title: 'Intro to Vim Tip #1'
url: /2006/07/10/139
views:
- "51"
---

Vim is a great tool, but using is can be a pita in the beginning- hence, we go through the basics. There are several command modes, but we’ll only discuss a few at first: Command Mode and Insert Mode.

Command mode is used to perform actions like saving, searching, etc. Insert mode is used to insert and delete text. You’ll be switching between them a lot.

Open a file from the cli:

```
  $ vim foo.php
```

change to insert mode from Command mode:

```
  press 'i'
```

change to command mode from insert mode:

```
  press [esc]
```

save from insert mode:

```
  press [esc]:w[enter]
```

save and quit from insert mode:

```
  press [esc]:wq[enter]
```

quit and do NOT save from insert mode:

```
  press [esc]:q![enter]
```

An there you have it.