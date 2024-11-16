---
author: Jesse Morgan
categories:
- Uncategorized
date: "2010-05-14T20:23:03Z"
guid: http://morgajel.net/?p=717
id: 717
tags:
- linkedin
title: Port Conventions
url: /2010/05/14/717
views:
- "125"
---

Over the past few years I’ve been doing a lot of work with JBoss and tomcat. One issue we’ve always had is bringing some level of sanity to ports that are in use. My current situation is somewhat abnormal; we have 12 applications, each with two instances, each with 7 ports- that’s a total of 168 ports that we need to keep track of. Now, multiply that by tomcat, apache and nagios’ configurations. Now multiply that by Dev, QA, ITL, PRJ, Preprod and Prod environments on segregated hardware. Even though a LOT of these ports can overlap, that’s a LOT to keep track of in a flat file or spreadsheet. We also require a certain level of flexibility:

1. Port Expansion: I mentioned there were 7 ports we use; originally there were only 6, but I enabled SNMP to better trend JVM usage (24 JVMs use a *lot* of RAM). As we add more features, more ports could be needed.
2. Application Expansion: We originally started out with 9 instances, and after 5 months added another three. More will be possible in the future.

The required flexibility makes numbering conventions difficult. The solution we came up with is to key the information and use it to build the port number. In order to do that, we had to make a few assumptions:

1. Environments are sequestered to their own hardware, however they’re on the same networks- i.e. dev and qa instances will never be on the same physical server, but will share multicast ranges
2. There will not be more than 5 instances in a cluster- any more than that would put theoretical ports out the 65536 range; besides, replication shouldn’t be past 4 nodes, right?
3. Ports below 1024 are off limits
4. We are constrained by IP addresses; one IP per server.

From here we could allow the following constraints:

1. Environments can use the same ports, EXCEPT for multicast- we don’t want QA and prod replicating data!
2. All multicast should use 1 for their Instance ID

With that in mind, port numbers will be laid out with the following structure: **XYYZZ**

X = Instance ID (e.g. 1-4)  
YY = application ID (Helium,Hydrogen,Lithium, etc )  
ZZ = Port Type (jmx, ajp, snmp, etc)

`<br></br>1 Instance1<br></br>2 Instance2<br></br>3 Instance3<br></br>4 Instance4<br></br>`

`<br></br>01 Hydrogen<br></br>02 Helium<br></br>03 Lithium<br></br>04 Beryllium<br></br>05 Boron<br></br>06 Carbon<br></br>07 Nitrogen<br></br>08 Oxygen<br></br>09 Fluorine<br></br>10 Neon<br></br>11 Sodium<br></br>12 Magnesium<br></br>`

`<br></br>01 Shutdown<br></br>02 JMX<br></br>03 HTTP<br></br>04 AJP<br></br>05 SNMP<br></br>06 REPL<br></br>07 MCASTDEV<br></br>08 MCASTQA<br></br>09 MCASTUAT<br></br>10 MCASTPREPROD<br></br>11 MCASTPROD<br></br>`

This allows us to define the following:

- Hydrogen AJP for instance 1 is 10104 (1+01+04)
- Oxygen SNMP for instance 4 is 40805 (4+08+05)
- Sodium Multicast for Prod 3 is 11111 (1+11+11) (remember, all multicast use 1 for their instance)

Other than the Multicast port kludge, this way seems to work pretty well. Numbers are easy to parse visually and can be maintained with a centralized key on the wiki. Anyone have any suggestions or examples of how they’ve solved this problem?