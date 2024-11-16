---
author: Jesse Morgan
categories:
- Uncategorized
date: "2005-12-19T09:27:00Z"
guid: http://morgajel.net/2005/12/19/38/
id: 38
title: redhats named
url: /2005/12/19/38
views:
- "51"
---

a question I had in IRC today… anyone want to take a shot?

08:21 ok, explain this to me.  
08:21 I wrote an iptables script that was pretty strict.  
08:22 it only allows tcp and udp on 53 for dns, and tcp for ssh. and related,established for all  
08:22 the defauly policy is to drop  
08:22 when I try to halt this redhat box, it locked up while trying to shut down Named  
08:23 so I on a fluke change the default policy to ACCEPT  
08:23 BAM it works  
08:23 so riddle me this…  
08:23 WTF is redhat’s /etc/init.d/named doing to lock up like that?

and yes, this is RHEL v3 with the latest updates.