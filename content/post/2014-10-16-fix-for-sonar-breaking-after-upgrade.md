---
author: Jesse Morgan
categories:
- Uncategorized
date: "2014-10-16T13:26:16Z"
guid: http://morgajel.net/?p=1571
id: 1571
title: Fix for Sonar Breaking After Upgrade
url: /2014/10/16/1571
---

> **Upgrade is not supported. Please use a production-ready database.**

If you’ve ever seen this message after a yum update, you know how infuriating it can be. I was sure I was already using mysql rather than the default h2 database, but everything indicated that was the problem.

It turns out the error was caused when they replaced **/opt/sonar/conf/sonar.properties** with a default configuration. If you vimdiff it against **/opt/sonar/conf/sonar.properties.rpmsave** you should see the issue.

Let me know if this helps.