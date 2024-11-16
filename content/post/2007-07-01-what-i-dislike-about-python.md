---
author: Jesse Morgan
categories:
- Development
- Open Source
date: "2007-07-01T10:19:09Z"
guid: http://morgajel.net/2007/07/01/208/
id: 208
title: What I Dislike About Python
url: /2007/07/01/208
views:
- "47"
---

Since I began working on the Luma project, I’ve been playing a lot with Python, a language that I’ve been around for years but never bothered to learn. Since Luma is written in Python and I’m not on the team, I figured it was time to jump in feet first. Coming from a perl/php/ruby/java background, it wasn’t a big leap to make.

However, the more I read and write, the less I like it. Some are minor complaints, while some are a bit larger. I’m gonna list them here as I think of them, from trivial to more important

\* **indentation** – This is really an annoyance more than anything. If you’re not aware, python is whitespace sensitive- rather than using curly braces, it uses indentation to show scope and what belongs where. Oh, and you better make sure to replace your tabs with spaces, otherwise you’ll ned up with bizarre errors that are a pain in the ass to track down.

\* **debugging tools** – This one is a bit more aggravating- I may not have found the equivalents to Data::Dumper or -w in perl, or .inspect in ruby, but that doesn’t mean they don’t exist. I’m sure I’ll find them in time. And for those of you who suggest using the python interpreter for debugging, that doesn’t really work that well when examining a GUI app.  
\* **self** – it seems really redundant that inside a class method, the first parameter you pass is self- in other languages I’ve used, self is so prevalent inside a class that it’s implicitly expected, not explicitly stated.  
\* **non-object old-style objects** – When is an object not an object? When it’s an old style python object. Apparently the Python object setup is sorta broken- new-style objects are subclassed from an actual object class, while old-style aren’t. People complain that Perl’s OO was tacked on, but this seems far more kludgy.  
\* **super** – related to the above point- new-style objects can use super, which requires being told which parent class it should be looking at (since python supports multiple inheritance). The problem is old-style classes do not, so if you’re subclassing a module you didn’t write which itself is subclassed, you may have to dig to figure out if you can use super or not. Old-style classes use a different method- you call ParentClass.method() and pass it self as the first parameter. I think this probably ties into the kludgy way self is handled. For more on super, check out [Python’s Super being Harmful](http://fuhm.net/super-harmful/) (which goes way over my head).

\* **a line fails but it doesn’t say why, it just stops**– I’ve run into this problem while trying to debug code- take the following code for example:  
`<br></br>            print "in workerthreadadd- will it add? 1"<br></br>            searchResult = self.ldapServerObject.add_s(self.dn, self.modlist)<br></br>            print "in workerthreadadd- will it add? 2"<br></br>`  
The first line will print- the second one won’t. I don’t know what’s going on in that ldapServerObject- I didn’t write it, but there’s not stackdump or errors spewing out- it just stops. Now I have to go through and debug the object I’m calling to figure out why it’s dying. (This isn’t the actual code that gave me this problem. I can’t remember what was doing it previously.)

That said I’m still gonna keep at it- I didn’t learn perl overnight, nor ruby. I suspect python will take a while to grow on me. When I learned php it was great compared to java because it didn’t need to be compiled; when I learned perl it handled text and gui interfaces better than php; when I learned ruby it’s OO abilities were far greater than perl’s. I just have to figure out what it’s good at that ruby and perl aren’t.

**update**:  
It looks like the old vs. new style objects will be gone in python 3.0- they’re planning on switching to new-style only. I think it’ll be a positive step overall, but after talking to the guys in #python, python 3.0 is much like Perl 6- it should be here sometime within the next decade, but I probably shouldn’t even narrow it down that much 🙂