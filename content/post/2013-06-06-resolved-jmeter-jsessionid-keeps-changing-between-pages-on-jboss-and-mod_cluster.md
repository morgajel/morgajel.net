---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-06-06T13:08:27Z"
guid: http://morgajel.net/?p=1425
id: 1425
title: 'Resolved: JMeter JSESSIONID keeps changing between pages on JBoss and Mod_Cluster'
url: /2013/06/06/1425
---

The problem isn’t with your server, it’s with your cookie manager.

Make sure you’re using the default Cookie Policy with the HC4CookieHandler implementation.

Leave a comment if this helped you out. it took an hour of searching to figure it out…