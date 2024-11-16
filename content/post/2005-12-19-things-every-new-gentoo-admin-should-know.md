---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Rant
date: "2005-12-19T06:25:56Z"
guid: http://morgajel.net/2005/12/19/37/
id: 37
title: Things every new Gentoo admin/user should know.
url: /2005/12/19/37
views:
- "72"
---

I’ve been using Gentoo for over 2 years now. Before that it was Debian. Before that it was Redhat. Spattered inbetween I’ve used slackware, mandrake, suse, knoppix, ubuntu, xandros and sorceror.

I’ve noticed when I pick up a new distribution, there’s always little bits and tips that people forget to tell you about. I’m gonna try to make a list for gentoo.

### emerge

Emerge is the main package management tool for gentoo- as such, there are several useful tips that may make your life easier.

- **emerge -up world** This is your basic command to update your system
- **emerge -uvpDN world** is what I use to update my system

As you can see, I use 3 extra flags, the –verbose flag, the –deep flag, and the –new flag. What do these do?

- **–verbose** One of the most useful flags you’ll ever use, it shows how big the download is (and hence the ability to make a guess at how long it’ll take). it also shows you which USE flags are available for a package for example, bitchx has a gtk flag, meaning it apparently has a gui version as well, which is something I didn’t know.
- **–deep** The deep flag is a blessing and a curse. emerge -up world only updates immediate dependencies, while –deep causes the entire tree to be checked. if you want to \*completely\* update your tree, this is how you do it. Here’s where the curse part comes in- if you’ve never used –deep before, be prepared for a bumpy ride. it sometimes breaks things, like library dependencies. These should be cleared up with a revdep-rebuild, which I’ll get to in a bit.
- **–new**The -N flag serves a purpose similar to –deep. when you add/change a USE variable, for one package, for example adding GTK so you can install bitchx with the gtk gui, several things happen. first, when you try to install bitchx after enabling this flag, it installs all the gtk-based packages that are needed to allow the gtk gui on bitchx. suppose later on you decide to update your system. well, it turns out many many other programs can use gtk now that the gtk flag is enabled. The -N flag brings them out of the woodwork. it should be noted that this works for both adding and removing flags from the use variable.

### revdep-rebuild

Sometimes when you update your system, you update to newer versions of libraries.  
so what happens to all the programs already installed and compiled to use libFlac.so.3 when you remove it and install libFlac.so.4?

well, they break sometimes. This is a common problem with using the -D flag when upgrading. how do you get around it? You run revdep-rebuild. revdep goes through a few phases- Collecting system binaries and libraries, Collecting complete LD\_LIBRARY\_PATH, Checking dynamic linking consistency, Assigning files to ebuilds, and emerging new packages.  
Basically it checks all your binaries for the libraries they use, then check to see if the libs exist. if not, it determines which packages containt the binaries looking for the missing packages and recompiles them. I usually watch this process very carefully and stop it after it identifies which libraries are breaking binaries. a ctrl-c will break out of this, but you have to make sure you remove the temporary files in root’s home dir, ~/.revdep.\*

This is a great way to find programs that slip through the cracks- for example, I’ve been using kde for a while now and recently upgraded my flac libraries. later, when I upgraded to kde 3.5, kde 3.4.3 was not removed. as I ran revdep-rebuild, it complained that kdemultimedia-3.4.3 was looking for the old version of flac. I ran  
emerge -vp unmerge kdemultimedia  
and sure enough it showed I had not only kde 3.4.3 and 3.5.0 installed, but 3.3.2 and 3.2.3 installed! I had left them in place “just incase” and each time forgot to remove the previous verson. I unmerged the old versions, removed the revdep temporary files and ran it again. everything was happy.

### perl-cleaner and python-updater

These two programs piss me off to no end. Perl-cleaner serves many functions, but the main on is it reviews all the emerged-installed perl packages and re-emerges them. useful for when perl is updated, but a shame it’s not more automatic.

Python-updater needs to be run when you update python. What happens if you don’t? well, nothing that uses python will work for one. no warnings, no messages nothing- just a blip that scrolls by in the middle of a world upgrade. I found this gem in the gentoo forums after the cedega installer “point2play” broke for no apparent reason.

well I’m sure this list will get longer, but this is all you’re getting for now. I probably shouldn’t have ended on a down note, so think about puppies or something. good luck and enjoy gentoo.

UPDATE 20051220-

Something else I should have mentioned when talking about the use flags and the -N flag: if you ONLY want a single package to have a flag enabled, and the default action for the rest, rather than adding the flag to the USE variable in /etc/make.conf, you can add it to /etc/portage/package.use .  
for example- suppose you only wanted bitchx to have the GTK flag. it WILL still requite all the gtk dependencies, but this way you don’t have to recompile openoffice, lame, mplayer, distcc, etc. to do this, you could add any of the following to /etc/portage/package.use:

net-irc/bitchx gtk  
=net-irc/bitchx-1.1-r1 gtk  
=net-irc/bitchx-1.1\* gtk

each one has a different level of granularity. The first options covers all versions of the package. The second version is very specific: only that version. the third version is a hybrid- any subversion of 1.1- useful in case they find a security hole that needs to be patched but doesn’t warrant a new version.

There are actually several different files that can go in /etc/portage and provide similar functionality and syntax, but serve different purposes.  
/etc/portage/package.use  
/etc/portage/package.mask  
/etc/portage/package.unmask  
/etc/portage/package.keywords

besides use, keywords is the second most useful- it allows you to accept keywords for certaint packages, allowing you to mix stable and unstable packages. Here’s an example that is somehwhat outdated- when kde 3.4 came out, I was too impatient to wait for it to be given stable status in gentoo(stable for the x86 platform is “x86”, unstable is “~x86”), so I added the following lines for each package:  
=kde-base/kdeedu-3.4.0 ~x86

and then when I went to emerge, the 3.4 version of the packages were available.