---
author: Jesse Morgan
categories:
- Uncategorized
date: "2012-02-02T11:37:44Z"
guid: http://morgajel.net/?p=1177
id: 1177
title: Perl shortcuts in Vim
url: /2012/02/02/1177
---

Since I’m home sick and trying to figure out how to do something productive with my time, I figured I might as well share something useful. The following are a few hotkeys I have set up in vim that I use when working on perl:

```
map <F2> : call PerlCritic()<CR>
map <F3> : call PerlCheck()<CR>

map <F4> :!perltidy -i=4 -ci=4 -syn -w --add-semicolons --indent-spaced-block-comments --closing-side-comments --cuddled-else --maximum-consecutive-blank-lines=2 -l=120 -wbb=" + - * / x \\!= == >= <= =~ \\!~  < > ? & =  **= += *= &= <<= &&= -= /= ?= >>= ??= //= .= \\%= ^= x="<CR>



func! PerlCritic()
    exec "w" "Save the file
    exec "!perlcritic --stern "expand("%:t")
endfunction

func! PerlCheck()
    exec "w" "Save the file
    exec "!perl -c %"
endfunction


```