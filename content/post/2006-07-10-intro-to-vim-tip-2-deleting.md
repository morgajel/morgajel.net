---
author: Jesse Morgan
categories:
- FreeBSD
- Linux
- Open Source
- Utility
- Vim
date: "2006-07-10T09:14:24Z"
guid: http://morgajel.net/2006/07/10/140/
id: 140
title: 'Intro to Vim Tip #2 (deleting)'
url: /2006/07/10/140
views:
- "45"
---

Deleting in vim can be done several ways- in insert mode, the delete key and backspace key perform as you’d expect them to, but what if you want more?

delete the character to the left of the cursor:

```
[esc]d[left arrow]
```

delete the character to the right of the cursor:

```
[esc]d[right arrow]
```

deleting the current line from insert mode:

```
[esc]dd
```

deleting the current line and the one below from insert mode:

```
[esc]d[downkey]
```

deleting the current line and the one above from insert mode:

```
[esc]d[upkey]
```

deleting current character and 4 to the right:

```
d5[rightkey]
```

deleting current line and 2 below:

```
d2[downkey]
```

You’ll notice from those last two examples that deleting characters to the left and right include the current character in the count, but deleting lines above and below do not. Weird.