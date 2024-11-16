---
author: Jesse Morgan
categories:
- IRC
date: "2005-10-14T11:09:22Z"
guid: http://morgajel.net/2005/10/14/17/
id: 17
title: Wall of Lamers
url: /2005/10/14/17
views:
- "41"
---

I wrote this script a while back, thought I’d share with the gentlemen in #asp. It works great if you’re on a \*nix box. It shows who has tried to !search, as well as how many times they tried it.

> \#!/bin/bash
> 
> LOGFILE=”/home/morgajel/.xchat2/xchatlogs/UnderNet-#asp.log”
> 
> \#the keyword  
> grep “!search” $LOGFILE | \\
> 
> \# remove the baiters and regulars- they know the score.  
> grep -iv ‘k\_f\\|subnet\\|morg\\|pytte\\|Crusher\\|lanshark\\|carlsb\\|dev.nu\\|LlamaHerder’ | \\  
> grep -iv ‘James\_But\\|vp.bofh\\|mylo\\|shedao\\|ram\\|phool\\|bebez\\|rochess\\|maestro’ | \\
> 
> \#make sure we don’t accidentally get the topic  
> grep -iv ‘Topic for’ | \\
> 
> \#remove all th text before and after their nickname  
> sed -e ‘s/\[^.\*//’| \\
> 
> \#sort, unique, and list the count.  
> sort|uniq -c