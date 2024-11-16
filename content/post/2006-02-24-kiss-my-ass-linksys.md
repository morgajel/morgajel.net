---
author: Jesse Morgan
categories:
- Electronics
- Rant
date: "2006-02-24T12:52:43Z"
guid: http://morgajel.net/2006/02/24/93/
id: 93
title: Kiss my ass, linksys
url: /2006/02/24/93
views:
- "61"
---

I remember, back in the day when Quality was more important than looking pretty… well, not really- marketing droids and slack-jawed fools always choose shiny and useless over plain and useful.

take, for example, the links WRT54G router. When I first got it, the firmware was buggy and ugly, but I could use it in lynx, a text browser. no pretty colors or images, just straight text. Using lynx meant I could edit my router config remotely to add/remove ports as I needed them.

well, that day is gone. it’s been gone for a while, but I recent had my hopes dashed again. The last revision of the firmware made the linksys router interface unusable in lynx (and elinks). I was messing with it yesterday when I got fed up and went in search of newer firmware for it- just my luck, I found it. Unfortunately, linksys took this oppertunity to make their interface even MORE useless.  
It’s so bad, that I thought I’d include a few screenshots.

![screencap of the linksys text interface](http://morgajel.com/screenshots/linksys.png)  
Here you can see the base interface I see when I log in- nothing special, but useful.  
Below you see what happens when you dig past this pretty facade. this is what happens when I go to the security page.  
![screencap of the linksys text interface showing only underscores and brackets- no text or labels](http://morgajel.com/screenshots/linksys1.png)

This is an EMBARASSMENT in my eyes. Linksys, you should be ashamed. I know this is an embedded device and all, but for crying out loud, could you at least get some competent developers in there? Keep in mind, you’re seeing what a Blind person would hear when they tried to access this with a text reader.

So who do we have to blame for this crappy code?  
![screencap of the linksys text interface source code showing it's property of another company and not for production use](http://morgajel.com/screenshots/linksys2.png)

And I quote, for the visually impaired: “This software should be used as a reference only, and it not intended for production use!”

Someone should tell Linksys that.

so I guess I could either load up an open source firmware binary and hope for the best, or just get a new router. whatever I get, it probably won’t be a linksys. I’m tired of this shit.

UPDATE: as mentioned in the comment below, the firmware version was showing what it was like BEFORE this latest update. this is what it looks like aver reloading the page:

![screencap of the linksys text interface showing only underscores and brackets- no text or labels](http://morgajel.com/screenshots/linksys3.png)  
It makes me want to vomit, seriously.