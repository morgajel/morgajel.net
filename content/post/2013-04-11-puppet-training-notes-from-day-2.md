---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-04-11T08:06:35Z"
guid: http://morgajel.net/?p=1390
id: 1390
title: Puppet Training Notes from Day 2.
url: /2013/04/11/1390
---

<span style="font-size: 0.8em;"> Notes from Day 2:</span>

1. **subscribe** is the other side of **notify** the same way **require** is for **before**.
2. In puppet.conf, under \[agent\] set **graph=true** for previously mentioned dot file creation.
3. I need to take a look at concat, file\_line and augeas.
4. Use **puppet parser validate init.pp** to quick validate syntax.
5. **audit=&gt;’content’** will track changes, even if puppet doesn’t care what it is.
6. Setting default values in a class can drastically reduce manifest size.
7. Use **${::hostname}** to access the fact **hostname** rather than a potentially overridden local value.
8. Use **path=&gt;’blah**‘ more to make **exec** command easier to read;
9. You can test fact conditionals with **FACTER\_operatingsystem=Debian puppet apply ../tests/init.pp**.
10. check out vagrant, looks useful.
11. You can use rubular to test regexes.
12. There is a presentation on using “profiles and roles” design pattern that’s worth rereading
13. Profiles can be treated as a collection of modules.
14. razor server is the provisioning tool that Joe mentioned, but it’s not ready for prod usage.
15. We can define a new resource type for vhosts and define them individually. I need to keep an eye out for other uses.