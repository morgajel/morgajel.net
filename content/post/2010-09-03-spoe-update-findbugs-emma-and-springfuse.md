---
author: Jesse Morgan
categories:
- Projects
- SPoE
date: "2010-09-03T20:10:39Z"
guid: http://morgajel.net/?p=946
id: 946
title: 'SPoE Update: findbugs, emma and springfuse'
url: /2010/09/03/946
views:
- "78"
---

So far,Â getting infrastructure in place has been much easier than actually doing any coding. As it stands now, I have added the following helpers:

- **Cobertura and Emma:** Code coverage.
- **Findbugs:** finding obvious bugs in my code
- **PMD:** Find other problems in the code.

So far that’s all I have- [bells and whistles to distract me from doing actual work](http://boron.morgajel.com/hudson/view/dash/job/SPoE%20Cont%20Build/). Ideally they’ll help keep me honest about code quality, but this early in the game they don’t offer much value.

As far as development goes, I found a neat little site called [springfuse](http://www.springfuse.com/) which, when given a database schema, reverse engineers an entire site for you. It seemed pretty impressive, and I began using that as a baseline for development… the only problem with springfuse was that it used antiquated packages. Short off falling back to Spring 2.5, and an older version of hibernate, it would be a royal pain in the ass to get it all functional.

I’ve stripped out most of the springfuse code at this point, only keeping the base Account class and surrounding classes. If it is salvageable, I may also try and save Role and AccountRole. I’m not saying there’s anything wrong with using springfuse- it seems like a great time saver for someone familiar with spring, but I think it would cause me me grief than effort it would save.

In the mean time I’ll be reading up on [spring security 3.0.x](http://static.springsource.org/spring-security/site/docs/3.0.x/reference/springsecurity-single.html#d0e1581), and hope to get a simple account model in place well enough to create accounts and log in. With any luck this long weekend will help.