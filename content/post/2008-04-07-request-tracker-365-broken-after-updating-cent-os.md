---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Work
date: "2008-04-07T10:59:18Z"
guid: http://morgajel.net/2008/04/07/241/
id: 241
title: Request Tracker 3.6.5 broken after updating Cent OS
url: /2008/04/07/241
views:
- "110"
---

 Can’t locate object method “seek” via package “File::Temp” at /usr/lib/perl5/site\_perl/5.8.8/MIME/Parser.pm line 816.

The underlying problem is perl was updated and overwrote the “correct” version of File::Temp that you probably installed when setting up RT and forgot about. To fix this issue

`<br></br>cpan install File::Temp<br></br>/etc/init.d/httpd restart<br></br>`

MAKE SURE TO RESTART APACHE! I didn’t, and it cost me probably 2 hours of screwing around with it.

I’m posting this because  
<http://www.nabble.com/RT-3.6.5-and-Sendmail-error-and-looks-like-perl-error-td15989015.html>  
Didn’t really mention what the final working solution was.