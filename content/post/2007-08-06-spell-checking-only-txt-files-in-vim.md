---
author: Jesse Morgan
categories:
- Vim
date: "2007-08-06T15:37:50Z"
guid: http://morgajel.net/2007/08/06/215/
id: 215
title: Spell checking only *.txt files in Vim
url: /2007/08/06/215
views:
- "184"
---

So vim 7 has native spell checking- which is great if you remember to turn it on. Enabling it in your .vimrc is fine if all you view is written text files, but annoying when it’s running while working on code.

That’s why I created this little snippet of sunshine:

add this to ~/.vimrc:

```

au BufNewFile,BufRead *.txt call ConfigureTxtFile()

func! ConfigureTxtFile()
    setlocal spell spelllang=en_us
    set wrapmargin=80
    set textwidth=80
endfunction
```

I also use it to set my text width and wrap margins. You can have it match other types of files as well and create new functions- I plan on using this to set .py files to use space-replaced tabs. As I go forward I might have it do something a bit more fancy.