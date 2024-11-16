---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-03-07T16:15:35Z"
guid: http://morgajel.net/?p=1370
id: 1370
title: Quick note on Mod_cluster, JBoss EAP and static content
url: /2013/03/07/1370
---

While trying to configure mod\_cluster for a new setup, I ran into a gotcha that got me good- first time I’ve ever seen apache httpd have an out of memory error in my 13 years of using it. The default behavior of mod cluster is to dynamically set up the basic context roots it receives from JBoss -i.e. foo.war has a context of /foo and mod\_cluster will forward it without much explicit configuration on the httpd side.

Suppose you want static content at foo/images/ to be served by httpd? The docs are very clear that you can do that with ProxyPassMatch, but what it isn’t clear on is that adding it removed the default context configuration, leaving you to explicitly state it. Not only that, but it has to come AFTER the ProxyPassMatch statements. Here’s what a full example should look like.

> ProxyPassMatch ^(/foo/css/.\*)$ !  
> ProxyPassMatch ^(/foo/docs/.\*)$ !  
> ProxyPassMatch ^(/foo/fonts/.\*)$ !  
> ProxyPassMatch ^(/foo/htc/.\*)$ !  
> ProxyPassMatch ^(/foo/html/.\*)$ !  
> ProxyPassMatch ^(/foo/images/.\*)$ !  
> ProxyPassMatch ^(/foo/js/.\*)$ !  
> ProxyPass /foo balancer://myclustername/foo

Note that this is sort of backwards behavior from mod\_jk and jkunmount. Hopefully this will help someone else. Leave a note if it does.