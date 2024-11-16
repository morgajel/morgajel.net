---
author: Jesse Morgan
categories:
- Uncategorized
date: "2009-03-30T23:07:46Z"
guid: http://morgajel.net/?p=471
id: 471
title: The Mixed Blessing of System Failure
url: /2009/03/30/471
views:
- "56"
---

ok, so here’s the scoop. I was planning on switching unicron over to CentOS from Ubuntu, but during the prep process, I must have bumped a sata cable on my raid array- the result was me rebooting, rebuilding the drive and shaking it off as a fluke.

A day later, that drive and another failed, which is bad news on a 4 disk raid5 array- usually it means everything is lost.

By some minor miracle, I was able to restore the array, but in the process installed cent into one of the two 1-gig swap partitions (formatted to ext3). From there I was able to make sure the data was backed up.

I had already created the 20 gig partition I planned to install CentOS to (it was unavailable during the raid failure, hence I didn’t use it), so I figured, “what the hell, the server is down I might as well rebuild it now.”

It’s a pain in the ass, but in the end it forced my hand to rebuild the server. The hardest part is remembering all the functionality I’ve set up over the last 2 years. Here’s what I’ve done so far since Friday:

- reinstall
- fix funky ext3 partition (/share drive)
- install screen, irssi, sudo which, lsof, lsusb
- install bind, dhcp
- configure bind, dhcp
- activate bind, dhcp
- shutdown linksys dhcp
- modify iptables to let traffic through
- install apache, mod\_ssl
- reconfigured apache, based on work config
- imported mysql DBs (including wiki)
- reinstalled wiki
- setup ssl certs
- set up unrealircd
- ssh keys working
- configure sshd
- set up ldap
- implement ldap into system accounts
- get ldapadmin working
- setup snmpd

I’ve done quite a bit more, but this is just what I remembered to document so far. What’s left?

- get subversion working
- set up myphpadmin
- set up /share share
- set up backups for LE websites
- set up nessus
- set up nagios
- set up nagiosgraph
- recreate ldap accts
- set up netflow/ntop

I’m sure this list will grow over the next few days.