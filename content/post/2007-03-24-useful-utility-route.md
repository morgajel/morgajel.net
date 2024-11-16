---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Utility
date: "2007-03-24T02:25:37Z"
guid: http://morgajel.net/2007/03/24/112/
id: 112
title: 'Useful Utility: route'
url: /2007/03/24/112
views:
- "49"
---

Route is one of these hate-inspiring, jaw droppingly obtuse programs that you always get the syntax wrong on. The purpose is simple enough- show and/or change the routing table. The most common uses are:

1. **route** – shows the current entries
2. **route add** – adds a new entry
3. **route del** – removes an entry
4. **route flush**– removes all entries

**Checking out your Routes**  
The simplest use of route is to simply run route at the command line:

```

morgajel@p-nut ~ $ /sbin/route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
192.168.0.0     *               255.255.255.0   U     0      0        0 eth0
loopback        *               255.0.0.0       U     0      0        0 lo
default         192.168.0.1     0.0.0.0         UG    0      0        0 eth0
```

You’ll see 3 routes total in this example (which is very simple)- The first route points all 192.168.0.x traffic to the network card(eth0), the second points all loopback traffic (127.x.x.x) back to the local device (lo), and anything that doesn’t fit into either of those categories goes to the network card (eth0). You may wonder why that first route is in there if the default would just catch it anyways- you see, this allows multiple network cards to point traffic to different gateways or routers on the same network.  
Take the next example:

```

morgajel@p-nut ~ $ /sbin/route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
10.100.0.0     10.100.0.1              255.255.0.0   U     0      0        0 eth1
192.168.7.0     192.168.7.1               255.255.0.0   U     0      0        0 eth0
loopback        *               255.0.0.0       U     0      0        0 lo
default         192.168.0.1     0.0.0.0         UG    0      0        0 eth0
```

all of your normal traffic would proceed to 192.168.0.1, but any traffic to 192.168.7.x would go to a different gateway(192.168.7.1) and all traffic to 10.100.x.x would go to a secondary network card(eth1) and be sent to yet another gateway (10.100.0.0). This would be useful if you were using a load balancing device like a Netscalar or F5, or if you had an internal network and a secondary DMZ’d network or something.

 **Adding and Removing Routes**  
if you’re manually setting up routes for a static IP, you’ll generally do something like

```

route add default gw 192.168.0.1
```

or remove it with

```

route del default
route del -net 192.168.0.0 netmask 255.255.0.0 eth1
```

Deleting routes is always the pain- the default route you can simply remove like the first example, but anything else needs the netmask specified and the -net flag used. If you want to see some more examples of routes, try

```

route -C
```

This will show you… uh, I guess the dynamic routes that have been recently used and cached. If you’re unsure about how to remove or add a route, you can run “route add” or “route del” without any addition parameters to see more options- I think the most I’ve used is something like

```

route add -net 10.100.32.0 netmask 255.255.248.0 gw 10.100.32.1 eth0
```

**Final Thoughts**  
One thing that I learned while writing this is that there is an additional parameter called “reject” which you can append to the end of an add line to basically form a crude firewall to that route ( note- it’s not really a firewall, but it will reject packets). And of course, to get rid of any line you’ve added, you can change an add to a del and it’ll probably remove it (you may need to remove some add-specific parameters like reinstate or mss).

The manpage is pretty straightforward and has some decent examples- Looking at it, I’m not sure why I’ve had such problems with route in the past. Overall it seems pretty simple as long as you get the del syntax correct. Overall I think writing this helped me more than it’ll probably help you.