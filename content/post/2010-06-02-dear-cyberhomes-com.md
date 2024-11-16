---
author: Jesse Morgan
categories:
- Uncategorized
date: "2010-06-02T20:57:50Z"
guid: http://morgajel.net/?p=729
id: 729
title: Dear Cyberhomes.com
url: /2010/06/02/729
views:
- "92"
---

Some day I hope you’ll join us in the future, where websites don’t fail miserably for having the wrong user-agent string. Here’s the problem- I go to check my home value at cyberhomes only to receive the following message on every single page:

> Your browser is not supported.
> 
> We do not support your browser yet…
> 
> If you are on a Windows machine please use:  
>  \* Internet Explorer 6 and up  
>  \* Firefox 1.5 and up  
>  \* Safari Beta 3 and up
> 
> If you are on a Mac please use:  
>  \* Safari  
>  \* Firefox 1.5 and up
> 
> Your current browser is Mozilla 1.9.2.5

There are several problems with this:

1. I am using firefox 1.5 and up, specifically, `"Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.9.2.5pre) Gecko/20100501 Ubuntu/9.10 (karmic) Namoroka/3.6.5pre"` I can only presume that since it doesn’t say “firefox”, it fails.
2. You’re parsing my connection string wrong-1.9.2.5 is related to my profile version, not browser version. Don’t do that.
3. Even after I change my connection string to Firefox/1.5, it STILL fails. The error message says Windows or Mac; maybe it’s failing because it is looking for one or the other.
4. The site is completely useless if that page comes up- no contact info to report a problem, nothing. I can’t even tell you that you’re accidentally blocking me.

I end up having to use the following connection string:  
`"Mozilla/5.0 (Windows; U; Windows NT 5.2; en-US; rv:1.8.0.8) Gecko/20061025 Firefox/1.5.0.8"`  
and the sad part was YOUR SITE LOOKED FINE. This kind of ungraceful degradation irritates the hell out of many people. If you don’t support my browser, I get it- you don’t want to give people a bad experience. You know what you do rather than make the site completely useless? You pop up a little window saying:

> Sorry, we do not support your browser yet…
> 
> For better browsing experience, we suggest you try the following:  
> If you are on a Windows machine please use:  
>  \* Internet Explorer 6 and up  
>  \* Firefox 1.5 and up  
>  \* Safari Beta 3 and up
> 
> If you are on a Mac please use:  
>  \* Safari  
>  \* Firefox 1.5 and up
> 
> Your current browser is Mozilla 1.9.2.5  
>  \[x\] continue anyways…

Note that the little \[x\] is a close button- VERY important on a warning like that. This allows the users to continue on viewing your website on the off chance that you accidentally mislabeled them.

This is 2010. This is the future. An issue like this was annoying back in 1998- there’s no reason a pretty site like yours should be doing this. Get with the program.

Sincerely,  
– Jesse