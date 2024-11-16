---
author: Jesse Morgan
categories:
- Books
- Hobbies
- Linux
- Open Source
- Website
date: "2006-11-13T00:08:07Z"
guid: http://morgajel.net/2006/11/13/170/
id: 170
title: Ruby on Rails
url: /2006/11/13/170
views:
- "51"
---

So I’ve had this on again, off again thing with ruby for a while now. Since I first started playing with ruby it got pretty big with rails, and I completely missed that boat. Well, now I’m playing with rails and it’s fairly interesting once you get it up and running. I picked up the O’Reilly book Ruby On Rails and have been walking through it’s Photo project. I’ve went so far as to even throw it in a subversion repository in case I pooch something.

One thing I really like so far is the Scaffolding system- once you create an object model (say, a Photo) and have it generate the table to store it in, it can auto-generate the web interface to allow you to create/edit/delete entries without having to muck with it. The coolest part is if you make any DB changes, the interface is automatically updated. That was something I’ve always hated- updating interface code to reflect DB changes.  
At DP I wrote a minor system to do something like that in ASP, but it was still pretty crappy (and dangerous). Dendrite had created a system at SPX called Skel which was hideous (his words), but did sorta the same thing. The main project I worked on there (before it was cancelled) was a replacement for Skel that was very similar to this, but for perl. Now I really wish I woulda kept up with ruby because had I known of rails, I could have essentially ported it to perl and saved a lot of time.

I’m hoping that the rest of rails is as cool as this scaffolding code. It’s taking me a while (5 hours broken up) to wrap my head around it, but it’s finally starting to make sense. I’m looking forward to finding out more about it.