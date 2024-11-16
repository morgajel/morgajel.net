---
author: Jesse Morgan
categories:
- Uncategorized
date: "2010-03-12T15:46:09Z"
guid: http://morgajel.net/?p=685
id: 685
title: Tomcat Symlink Fun
url: /2010/03/12/685
views:
- "56"
---

Here’s how I stumbled across this (names changed to protect the guilty):  
Suppose you have 4 tomcat instances, A1, A2, B1 and B2. A1 and A2 run application Apple/. B1 and B2 run application Breakfast. Someone decided “Hey, we can save deployment time if we symlink A1 and A2 to the same directory.”

Obvious questions aside (why bother setting up two if they’re going to be hosting the same content on the same box?), lets look at what happens when you misconfigure this relatively simple idea. The tomcat instances each had a webapps directory where their application was deployed, for example,Â *A1/webapps/Apple/* or *B2/webapps/Breakfast/*. The goal was to create a common deployment location for A1 and A2, B1 and B2, however a shortcut was taken and the webapps directory was symlinked rather than Apple and Breakfast, then BOTH Apple and Breakfast were placed in the shared directory. This resulted in 2 things:  
\* a unified place to deploy code  
\* every single tomcat instance loading BOTH Apple and Breakfast

This means *B1/webapps/* now includes *Apple/*. While this may not seem like a problem (B2’s context root is set to *Breakfast/*, for example), it can lead to complications down the road:  
\* each instance now needs to load and compile things for both applications, resulting in wasted resources and startup time.  
\* If *Breakfast/* happens to include it’s OWN *Apple/* (because apples are part of a complete breakfast,) *B2/webapps/Breakfast/Apple/* is now ignored in favor of *B2/webapps/Apple/.*

Fortunately I’d stumbled across this misconfiguration the day before others did, so I was able to quickly deduce what was wrong. Why didn’t I fix it? Because I wasn’t sure I was right, and I didn’t want to break someone elses apparently broken configuration any further. Besides, I’m trying to fight this obsessive feeling that “if I don’t do it myself, I won’t know if it’s right,” and doublechecking everyone’s work feeds that.

Fortunately the fix was quick and simple. It may not seem like that big of a deal, but you must remember this is a simplified view of the situation. the REAL setup had 10 applications, with some weighing in at 4-8 gigs each. While most of that is content (images and flash that tomcat doesn’t have to process), sorting through an extra 600k files \*per project\* will slow the system down.

Now, this behavior may seem dumb until you think of it from a tomcat point of view. “I know you have this /register/ under your context root, but you also have this application named register/, so I’m gonna go ahead and presume you want to run the full application rather thank this dinky directory. Besides, it’s probably a ‘sorry, registration is currently out of order’ in case the full application is taken offline.”