---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Reviews
- Utility
date: "2006-02-23T16:29:44Z"
guid: http://morgajel.net/?p=52
id: 52
title: 'Useful Utility: screen'
url: /2006/02/23/52
views:
- "46"
---

Screen is probably one of the top 10 most useful programs in the unix world- why? Because of what it does. Screen lets you create a session on a machine and then disconnect, while the session stays open. Suppose you wanted to start a large compile on your home server before you left work, but needed to shut down your laptop and bring it home.

You could ssh into the machine and simply start the compiling, but the compilation would stop when you broke the ssh connection by shutting off the laptop. That’s not acceptable. Enter screen.

###  Beginning with Screen

When you ssh to the server, instead of immediately running the compile, run screen.

```

morgajel@lappy ~ $ ssh example.com
morgajel@homeserver ~ $ screen
```

The terminal should flicker, clear, and show a regular prompt. you are now in screen. You can check this by using one of the many screen control commands.  
Go ahead and execute your compilation, then press ctrl+a followed by the ‘d’ key.  
The screen should flash and look something like this:

```

[detached]
morgajel@homeserver ~ $  
```

The session still exists, it’s just not attached to this particular terminal. go ahead and log out of homeserver and take your laptop home with you. As a side note, get used to pressing ctrl+a. That’s how you’re gonna communicate with screen.

When you get home, hop on your homepc and ssh into your homeserver.once you’re back in, run “screen -r -d”:

```

morgajel@homepc ~ $ ssh example.com
morgajel@homeserver ~ $ screen -r -d
```

There’s your compile- still running. It was running the entire time you had disconnected. Pretty cool, huh?

### Multiple Screens

Now lets try something a little more advanced- lets open another terminal inside this screen instance. while watching your compile, press ctrl+a followed by the ‘c’ key. This creates a new terminal. if you press ctrl+a followed by a double quote (“), you’ll see a list of existing terminals. To move to another terminal, you can highlight it and press enter or press ctrl+a followed by it’s id number. By far the most useful is ctrl+a followed by an ‘a’ which bounces you back to the previous terminal.

Just to give you an idea of how I use screen, it’s not uncommon for me to sit at work with a single ssh connection to my home server and branching ssh connections to each box at home. My current screen at the time of this writing looks like this (I’m updating my systems):

```

0: irc connection to freenode
1: irc connection to morgajel.com
2: irc connection to undernet.org
3: 'scratch' shell on p-nut
4: emerging packages on p-nut
5: emerging packages on pablo
6: emerging packages on draccus
7: emerging packages on jaxon
8: emerging packages on zaven
```

###  Meta Screens

There are also a couple of “meta” screens- screens you can get to that already exist. Ctrl+a followed by a double quote(“) as we discuessed earlier will bring up a list of all open screens.  
Ctrl+a followed by a question mark(?) will bring up a help menu.

… Actually, there’s not as many as I thought there were.

the man page as a nice breakdown of what ctrl+a options there are, as well as using “commands”, which are way out of the scope of this review.