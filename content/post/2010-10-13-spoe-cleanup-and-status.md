---
author: Jesse Morgan
categories:
- Projects
- SPoE
date: "2010-10-13T17:21:23Z"
guid: http://morgajel.net/?p=977
id: 977
title: 'SPoE: cleanup and status'
url: /2010/10/13/977
views:
- "81"
---

I’ve been relatively quiet on SPoE because I’ve been focused. Users can now register, log in, log out, and view existing snippets. I’ve added a jquery navigation menu which looks slick in firefox but ugly in IE 7. I’ve also added wymEditor for adding snippets, despite the fact that it doesn’t save; I’m not sure if it’s the final choice, but it’ll work for now.

The card system was a complete catastrophe;Â I was too focused on learning the framework and building out parts as I figured it out. I’ve now reached a point where I have several tasks I can complete, so perhaps cards will be more useful.

The Wireframe Sketcher tool has been very useful in helping me visualize what needs to be created. The writer of the plugin deemed my previous open source work good enough to grant me a free license, to which I’m thankful.

Oh, and I switched from JBoss to Tomcat for development. Logging is broken and more of a pain, but the restart times make it worth the switch. This also allows me to get rid of my defaultDS config.

As always, you can check the status via [Hudson](http://boron.morgajel.com/hudson/), or play with the site at <http://spoe.morgajel.com/> using the credentials jmorgan:hello. I wipe the db and deploy regularly, so don’t be surprised if things don’t always work.

Next tasks to work on:

- I want to be able to change passwords and other profile information \[<span style="color: #008000;">done</span>\]
- I want to be able to reset passwords via email \[<span style="color: #008000;">done</span>\]
- I want tomcat to properly log errors \[<span style="color: #008000;">done</span>\]
- save edited snippets \[<span style="color: #008000;">done</span>\]
- set up testng/integration testing