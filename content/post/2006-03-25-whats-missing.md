---
author: Jesse Morgan
categories:
- FreeBSD
- Open Source
- Rant
- Reviews
- Work
date: "2006-03-25T18:05:00Z"
guid: http://morgajel.net/2006/03/25/118/
id: 118
title: What&#8217;s Missing?
url: /2006/03/25/118
views:
- "51"
---

So, I’m compiling a list of what’s missing from my BSD install from the get go.

- **tab-complete** – stupid default shell is csh, which means no tab complete. Come on guys, jump on up to 1999.
- **alt key** – This is probably a keymap issue, but the alt and delete keys do not work. Alt acts like it does nothing, and delete behaves like a tilde. This means no alt tab. The question becomes, “if USA ISO is not the proper keymap, what is?” I’d like the one that windows and linux use as default if anyone knows offhand.
- **Where’s my X** – installed as a “user + X” setup, but X does not start on boot. I can figure out how to get it to start, but still a pain in the ass.
- **searching in sysinstall** – again a comparison to make menuconfig- searching in the package list with / would be really handy.
- **Scroll Wheel** – stupid scroll wheel doesn’t work by default
- **cvsup not installed** – I’m lead to believe that cvsup is responsible for the same effect as apt-get update or emerge sync in the linux world- i.e. major importance for package management. Then why isn’t it installed by default?
- **vim is not vi** – Perhaps this is another keymap related issue, but vim seems to think it’s vi- i.e. simple things like backspace and delete do not work properly in insert mode. I don’t know what’s going on, but I had to install nano to make even simple changes, and I’ve been using vi for years.
- **Updatedb** – There’s a locate, but no updatedb. Upon further research, I found a /usr/libexec/locate.updatedb. why it’s here, who knows, but it’s a pain in the ass- especially if you install a new system and you don’t want to wait a week for it to rebuild it’s database. Just one more thing to track down.

I know a lot of these arguments are of the vein “But that’s now how linux does it!”  
Yes, I know, BSD isn’t linux, and yeah you could probably argue it’s apples to oranges or that BSD is more secure. But these are simple things. And to those who argue that BSD has better documentation than Linux, I suggest you check out the gentoo project’s documentation- it somehow manages to cover exactly what you need and does it in an efficient manner.

For cryin out loud guys, being “a real unix” doesn’t mean you have to stagnate.