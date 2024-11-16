---
author: Jesse Morgan
categories:
- Development
- Open Source
- Rant
date: "2007-07-22T08:39:36Z"
guid: http://morgajel.net/2007/07/22/212/
id: 212
title: Supporting Standards vs. Supporting all Testcases
url: /2007/07/22/212
views:
- "71"
---

So I’ve been working on a side app that connects to remote servers. While writing validation code for inputs, I came across an interesting dilemma. I’d like to validate an address input to be valid- be it a hostname, domainname, or ip address. Unfortunately it’s not as simple as it sounds.

According to the [domain name RFC, section 3.5](http://tools.ietf.org/html/rfc1034") domain names must start with a letter, end with a letter or number, and can have letters, numbers or hyphens in between. When you add in periods to separate it out it’s messy, but workable.

But not everyone follows the RFC- take fifth/third bank for example- I’m not sure how they got away with it, but it doesn’t really follow this (historically I think 3com is the one to blame).

So that leaves me with two choices: follow the standard, or allow for the outliers. It’s doubtful anyone will be using my app to manage 53.com’s ldap server, and less likely that 53.com IS an ldap server. It is however possible that some one out there has either named or inherited a server named 3app6ldp02.foo.internal.

So do I support the guy? Should I care? should I even bother validating the address?