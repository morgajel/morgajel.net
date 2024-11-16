---
author: Jesse Morgan
categories:
- Uncategorized
date: "2007-03-19T10:26:22Z"
guid: http://morgajel.net/2007/03/19/183/
id: 183
title: Intro to apt-get
url: /2007/03/19/183
views:
- "38"
---

This article was originally written for draccus.net, then moved to morgajel.com. I’ve updated it a bit here to make it a bit more modern before posting it to my employers bbs.

Apt-get is a great package management utility created for the Debian Linux distribution. It can be found on children distributions like Ubuntu, Linspire, and mepis. For those who are not familiar with it, let me give you a quick run down.

**apt-get update**  
Apt-get works by maintaining a list of packages located on each site. To make sure you have the latest package list, you would run:  
`woof:~# apt-get update<br></br>`  
Apt-get will then go to each site listed in the /etc/apt/sources.list file and get the newest list of packages stored at it’s site. After downloading each list, it will update it’s own internal database. It’s important to run this command before installing or searching for any new packages. It also lets apt-get know when new packages are out to replace older ones on your system. If you plan on installing several packages in the same day, you only have to run this once.

**apt-get upgrade**  
Apt-get upgrade automatically updates any packages on your system to the newest known package. This is why it is important to run apt-get update before running apt-get upgrade. When newer packages are found, it asks if you wish to upgrade. These updates could be newer versions of the program, or the same version with security fixes included.

It’s a general rule of thumb to make sure you keep the packages updated as much as possible. I generally use the combination of apt-get update/apt-get upgrade on my systems once or twice a week; however, DO NOT automate this process. Apt-get has been known to have problems in the past, and the last thing you want is to have your entire network upgrade at 4am and have every single machine die at the same time. Like most upgrades, make sure there’s a live person there to troubleshoot and answer installation questions.

**apt-get dist-upgrade**  
Apt-get dist-upgrade is very similar to apt-get upgrade, except it has the power to make MAJOR upgrades- things that you might not want to do right away. An example would be the leap from KDE 2.2 to KDE 3.1, or from Debian 2.6 to Debian 3.0. You should consider very carefully before doing this. I usually check for a “distribution-upgrade” once a month. Again, this is NOT something you should automate.

**apt-get install**  
Apt-get install is the function you’ll probably use the most. By specifying the name of the package, you can tell apt-get to automatically download and install the package and any dependencies it might have. Let’s take the program xjewel for example. It is a puzzle game that is a clone of the popular arcade game Columns.  
`<br></br>woof:~# apt-get install xjewel<br></br>Reading Package Lists... Done<br></br>Building Dependency Tree... Done<br></br>The following NEW packages will be installed:<br></br>  xjewel<br></br>0 packages upgraded, 1 newly installed, 0 to remove and 2  not upgraded.<br></br>Need to get 28.4kB of archives. After unpacking 135kB will be used.<br></br>`

Apt-get checks it’s database to make sure that the package exists, then heads to it’s source site to download the game, grabbing any needed packages along the way.  
Installing packages can be a little more difficult. A good example is the package bsdgames-nonfree. I currently have the package bsdgames installed, however when I try to install bsdgames-nonfree, I get the following message:  
`<br></br>woof:~# apt-get install bsdgames-nonfree<br></br>Reading Package Lists... Done<br></br>Building Dependency Tree... Done<br></br>The following packages will be REMOVED:<br></br>  bsdgames<br></br>The following NEW packages will be installed:<br></br>  bsdgames-nonfree<br></br>0 packages upgraded, 1 newly installed, 1 to remove and 2  not upgraded.<br></br>Need to get 0B/226kB of archives. After unpacking 1864kB will be freed.<br></br>Do you want to continue? [Y/n]<br></br>`  
These packages have a “conflict,” and cannot be installed at the same time. This is very common for packages that are almost the same, but slightly different- such as different versions of the same program. The default action is to ask you if you want to remove the conflicting programs and any programs that are dependent on it.

Apt-get install has many helpful flags you can use for different circumstances. Here is a short list:

- -f fixes broken dependencies. In case of emergency, try this.
- –fix-missing downloads and installs any missing or broken packages.
- -y yes to all questions. useful for scripting, but you better know exactly what it will do.
- –build compiles packages that were downloaded as source.
- –reinstall reinstalls a currently installed program

it’s important you get comfortable with -f, –fix-missing, and –reinstall. They can provide valuable help when troubleshooting a broken package. More flags can be found by reading the manpage of apt-get.

**apt-get remove**  
Apt-get remove is the exact opposite of apt-get install. It removes the packages listed if they exist as well as packages dependent on the one you wish to remove. This will remove the program, but not the config files. If you wish to remove config files and any other related data, you can use the –purge flag.

**apt-get source**  
I have only used apt-get source once, so I may not know about all of it’s features. From what I’ve seen, it works similarly to apt-get install, but instead downloads the source files, allowing you to make modifications to the software before installing it. This will only be useful to a few people, and is generally not used by the average person.

**apt-get build-dep**  
Used with apt-get source, apt-get build-dep downloads and installs dependencies for the programs downloaded by apt-get source. Haven’t really used this feature much either, so my knowledge is rather limited.

and that about sums it up- there are many more features like pinning that I just don’t feel like going into, but feel free to leave extra help in the comments and I’ll tack it on to this.