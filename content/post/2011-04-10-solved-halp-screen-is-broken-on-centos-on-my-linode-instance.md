---
author: Jesse Morgan
categories:
- Linux
- Utility
date: "2011-04-10T02:51:09Z"
guid: http://morgajel.net/?p=1066
id: 1066
title: 'SOLVED: Halp, &#8220;screen&#8221; is broken on CentOS on my Linode instance!'
url: /2011/04/10/1066
views:
- "254"
---

So while migrating some content to a Linode instance, I attempted to fire up screen ran face first into a brick wall. For context, this was on an up-to-date CentOS 5.5 instance running screen-4.0.3-3.el5. Steps to reproduce:

- logged in as my normal user
- typed in “screen” and hit enter
- console went blank for 5 seconds
- previous content returned, along with “\[screen is terminating\]” and my normal command prompt.

Screen didn’t work for any regular users, however it did work for root.

I reinstalled the package, compared it to another centOS server, compared configs checked strace, etc. I could find no reason for this behavior.

Eventually I noticed I was using ttyp0 rather than pts/0 for my TTY; It was the only difference I could see between the two CentOS servers. Since I’d never seen ttyp (that I recall), it struck me as weird. I wasn’t sure if this was related to the fact that the broken instance was a xen guest or what.

It turns out, when Linode builds their CentOS image, they cut the following line out of /etc/fstab:

> /dev/ptmx /dev/pts devpts defaults 0 0

That prevents pts from existing, causing you to fall back to the older ttyp, which is apparently incompatible with screen.

once I got that puppy mounted, everything was good.