---
author: Jesse Morgan
categories:
- Books
- Linux
- Utility
date: "2006-06-21T15:57:57Z"
guid: http://morgajel.net/2006/06/21/134/
id: 134
title: recursing vimrc
url: /2006/06/21/134
views:
- "102"
---

I use vim a lot. a \*LOT\*. One thing that really annoys me is page width. When I’m writing code, I like to have a width set to 78 characters. But in some instances, say when I’m working on a book, I like the width set to 90 characters since it’s easier to read. This got me thinking… if I had vim check the current directory for a config, I could have custom configs for different directories. so in ~/.vimrc, I added

```

if filereadable("./.vimrc")
        source ./.vimrc
endif
```

and this works great, with one exception… what if i’m sitting in my home directory?

```

morgajel@p-nut ~ $ vim .vimrc
Error detected while processing /home/morgajel/.vimrc:
E169: Command too recursive
Hit ENTER or type command to continue
```

it goes into a recursive loop… so now my question is, how do I prevent that? I’m presuming that “changing the if statement to also check and make sure the current directory isn’t home” is the way to go, but I’m not seeing much documentation on how to go about doing it, or at least I’m not looking for the correct thing. Any suggestions?