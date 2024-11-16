---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Reviews
- Utility
date: "2019-09-26T21:00:30Z"
guid: http://morgajel.net/?p=59
id: 59
title: 'Unfinished Drafts: Useful Utility: cat'
url: /2019/09/26/59
---

***This article was originally written back on Feb 21st, 2006. While never completed, I thought it was worth sharing.***

Cat is a very simple utility- so simple I debated added it to this list. There are however three really useful flags. I’ll try to write as much as I can about it so you don’t feel ripped off by this article. hrm… did that last sentence sound like filler? I swear it wasn’t meant to- that’s completely on accident.

So what is cat? Cat is a utility for printing the contents of a file or files to the screen. for example:

```
morgajel@FCOH1W-8TJRW31 ~/docs $ cat path.txt
paths


database admin

system admin

network admin management
morgajel@FCOH1W-8TJRW31 ~/docs $ 
```

You can also specify several files as well if you want to train them all together and pipe them to another utility.

```
morgajel@FCOH1W-8TJRW31 ~/docs $ cat foo.log bar.log baz.log |grep "Invalid user"> invalid_users.txt
```

### Cat Flags

So there are three useful flags for cat. the first one is -n, which adds a linenumber to the output, like so:

```
morgajel@FCOH1W-8TJRW31 ~/docs $ cat path.txt -n
     1  paths
     2
     3
     4  database admin
     5
     6  system admin
     7
     8  network admin management
morgajel@FCOH1W-8TJRW31 ~/docs $
```

This can be useful when debugging source files. The next option is somewhat related; the -b option adds a line number, but only to non-blank lines. if you’re wanting to figure out for some reason what the 5th item is, not including blank lines, this is the way to go. Here’s an example of what it would look like:

```
morgajel@FCOH1W-8TJRW31 ~/docs $ cat path.txt -b
     1  paths


     2  database admin

     3  system admin

     4  network admin management
morgajel@FCOH1W-8TJRW31 ~/docs $
```

Notice how it only counted to 4? There were only 4 text lines. The final option that may or may not be of use is the -s flag, which smushes (that’s a technical term) blank lines together- it leaves single blank lines alone, but if there’s more than one blank line next to each other, it removes all except one. using our file above, watch what happens between “paths” and “database admin” in our example:

```
morgajel@FCOH1W-8TJRW31 ~/docs $ cat path.txt -s
paths

database admin

system admin

network admin management
morgajel@FCOH1W-8TJRW31 ~/docs $
```

Notice how there is only one blank line? That’s what -s does. if you’ve ever had a file where you’ve systematically removed text but not newlines and end up with a 500 line file with 20 lines of text, this can be useful for making it readable on a single page.

Well, that’s all I can really say about cat. If you have anything else to add, do so in the comments.