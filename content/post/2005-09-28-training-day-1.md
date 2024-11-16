---
author: Jesse Morgan
categories:
- Linux
- Work
date: "2005-09-28T06:40:25Z"
guid: http://morgajel.net/2005/09/28/8/
id: 8
title: Training, Day 1
url: /2005/09/28/8
views:
- "37"
---

Alright, yesterday was a long day. training started at 8:30am, went till 5:30pm. What did I learn?

That’s a damn good question.

I learned how to use a web interface for extremely complicated tracking program . My biggest complaint is this training cannot be abstracted to be useful on other projects; it’s basically a 2 day training course on an obscure application.

What I liked:

- clean interface, worked in both Firefox and IE.
- Uses XML for data storage; no messy binary data
- extensible
- runs on suse, RH, solaris, aix, etc. will probably run on debian, which I’d prefer.

What I didn’t like:

- JSP. This shouldn’t matter normally, but if you hit the back button in some areas you got a JSP error. It felt incredibly limited, on par with my other experiences with JSP sites (our internal resume system that’s JSP, michwork’s job search). You can’t bookmark pages, either (that I saw).
- incredibly complicated. While the interface was clean, the process was not. there was a lot of jumping around and some of the \*cough\* older members of the training group were lost easily. The other young guy, (name removed by request) I think, and I were both able to keep up for 95% of the session. The only mistakes I made were from glazing over a section, not from losing the entire process. This will be too complicated for most of SPX’s employees.

I gave their tech one implementation suggestion. The system sends out emails to “clients” saying if you have any questions, “respond to blah@blahblah”. However, it sends this email from a bunk email address at it’s server. If you hit reply, it replies to the bunk server email address, which bounces and freaks you out. It should put blah@blahblah in the reply-to field “just in case” someone doesn’t follow the directions and hits reply. It’s a small thing, and seems unimportant, but one of the less technically saavy users did exactly that and became really confused when he thought he was following directions and his email bounced back as undeliverable.

But anyways, seconds day starts today- this is only supposed to go till noon, and mostly be a Q&amp;A session. I’ll let you know how that goes.

Oh, one more thing- I get to go through special training since I’ll be the sys admin. That’s why this is in the Linux category as well.

-morgajel