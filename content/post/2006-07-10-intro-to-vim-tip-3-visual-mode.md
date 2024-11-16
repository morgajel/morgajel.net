---
author: Jesse Morgan
categories:
- FreeBSD
- Linux
- Open Source
- Utility
- Vim
date: "2006-07-10T09:38:19Z"
guid: http://morgajel.net/2006/07/10/141/
id: 141
title: 'Intro to Vim Tip #3 (Visual Mode)'
url: /2006/07/10/141
views:
- "63"
---

Another well used mode is Visual Mode, which turns your cursor into a hilighter.  
open a textfile with several lines of text ad move the cursor to the middle

switch from command mode to visual mode:

```
v
```

You’ll notice as you move the cursor around, you highlight different sections from the point you started to the point you left. you can press \[esc\] to return to command mode.

hilight a few lines of text from command mode:

```
[shift]v[upkey]
```

my adding the modifier \[shift\] when pressing V, you switch to ‘visual line mode’. This allows you to copy paragraphs easily.

hilight a block of text from command mode:

```
[ctrl]v[upkey][upkey][rightkey]
```

Now what good is visual mode? we” for deleting or replacing, of course! This is great when you have the following block of text:

```

           [option min="1"  max="10"   ][/option]
            [option min="11" max="19"   multiplier="10000" ]cp[/option]
            [option min="20" max="38"   multiplier="1000"  ]sp[/option]
            [option min="39" max="95"   multiplier="100"   ]gp[/option]
            [option min="96" max="100"  multiplier="10"    ]pp[/option]
```

and you’ve decided you no longer need min and max.  
– move the cursor in command mode over the ‘m’ in ‘min’ on the first line  
– press \[ctrl\]v\[downkey\]\[downkey\]\[downkey\]\[downkey\]  
– press the right arrowkey until the closing quote on “100” is covered  
– press ‘d’  
all your text will be gone. But suppose you didn’t want the text to be gone, you wanted to replace it with something else?  
– press ‘u’ and the text will reappear  
– re-highlight it and press ‘c’ (text should disapepar)  
– type your replacement string (use=”false”) and press \[esc\]  
within a moment or two, use=”false” should jump up across all five lines

Visual modes are also useful for narrowing the scope of a search and replace, but I’ll get to that when I cover search and replacing.