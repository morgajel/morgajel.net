---
author: Jesse Morgan
categories:
- Uncategorized
date: "2012-03-25T18:04:08Z"
guid: http://morgajel.net/?p=1208
id: 1208
title: The Limoncelli Test
url: /2012/03/25/1208
---

For grins, I went through Tom Limoncelli’s sysadmin questionaire to see how “a team I have worked with previously” fares:

- [A](http://everythingsysadmin.com/the-test.html#subA). Public facing practices:

- **[\*1](http://everythingsysadmin.com/the-test.html#ticket). Are user requests tracked via a ticket system?** <span style="color: #ff0000;">No, a high estimate would be 1/3rd of their requests are tracked.</span>
- **[\*2](http://everythingsysadmin.com/the-test.html#3policy). Are “the 3 empowering policies” defined and published?** <span style="color: #ff0000;">No.</span>
- **[3](http://everythingsysadmin.com/the-test.html#metrics). Does the team record monthly metrics?** <span style="color: #ff0000;">No. Outages are tracked by management, but that’s it. No alert stats, system-usage stats, etc.</span>

4. [B](http://everythingsysadmin.com/the-test.html#subB). Modern team practices:
- **[\*4](http://everythingsysadmin.com/the-test.html#wiki). Do you have a “policy and procedure” wiki?** Yes, although admittedly it is missing quite a bit.
- **[5](http://everythingsysadmin.com/the-test.html#pwsafe). Do you have a password safe?**<span style="color: #ff0000;"> No.</span>
- **[6](http://everythingsysadmin.com/the-test.html#sccs). Is your team’s code kept in a source code control system?** Most if not all, yes.
- **[7](http://everythingsysadmin.com/the-test.html#bugtrack). Does your team use a bug-tracking system for their own code?** <span style="color: #ff0000;">No.</span>
- **[8](http://everythingsysadmin.com/the-test.html#stabil). In your bugs/tickets, does stability have a higher priority than new features?** <span style="color: #ff0000;">N/A</span>
- **[9](http://everythingsysadmin.com/the-test.html#ddoc). Does your team write “design docs”?** <span style="color: #ff0000;">No. We have a few, but it’s not S.O.P.</span>
- **[10](http://everythingsysadmin.com/the-test.html#postmort). Do you have a “post-mortem” process?** Yes, each week we do one for the previous week’s oncall.

6. [C](http://everythingsysadmin.com/the-test.html#subC). Operational practices:
- **[\*11](http://everythingsysadmin.com/the-test.html#opsdoc). Does each service have an OpsDoc?** <span style="color: #ff0000;">No.</span>
- **[\*12](http://everythingsysadmin.com/the-test.html#monitor). Does each service have appropriate monitoring?** <span style="color: #ff0000;">No, probably only 60-70% coverage.</span>
- **[13](http://everythingsysadmin.com/the-test.html#rotation). Do you have a pager rotation schedule?** Yes. one out of every 9 weeks we are oncall.
- **[14](http://everythingsysadmin.com/the-test.html#qaprod). Do you have separate development, QA, and production systems?** Yes, we have dev, qa,stage and prod.
- **[15](http://everythingsysadmin.com/the-test.html#canary). Do roll-outs to many machines have a “canary process”?** <span style="color: #ff0000;">No.</span>

8. [D](http://everythingsysadmin.com/the-test.html#subD). Automation practices:
- **[16](http://everythingsysadmin.com/the-test.html#cfgmgmt). Do you use configuration management tools like cfengine/puppet/chef?**<span style="color: #ff0000;"> No, but I am working on implementing Puppet for our new builds.</span>
- **[17](http://everythingsysadmin.com/the-test.html#roleacct). Do automated administration tasks run under role accounts?**<span style="color: #ff0000;"> No.</span>
- **[18](http://everythingsysadmin.com/the-test.html#rolemail). Do automated processes that generate email only do so when they have something to say?** <span style="color: #ff0000;">No, but this has greatly improved.</span>

10. [E](http://everythingsysadmin.com/the-test.html#subE). Fleet management practices:
- **[\*19](http://everythingsysadmin.com/the-test.html#machdb). Is there a database of all machines?** Yes, LDAP Inventory
- **[20](http://everythingsysadmin.com/the-test.html#autoos). Is OS installation automated?** Yes, and we are improving it.
- **[\*21](http://everythingsysadmin.com/the-test.html#patch). Can you automatically patch software across your entire fleet?** <span style="color: #ff0000;">No.</span>
- **[22](http://everythingsysadmin.com/the-test.html#fleet). Do you have a PC refresh policy?** <span style="color: #ff0000;">No, presuming we’re talking about Servers.</span>

12. [F](http://everythingsysadmin.com/the-test.html#subF). “We acknowledge that hardware breaks” practices:
- **[\*23](http://everythingsysadmin.com/the-test.html#raid). Can your servers keep operating even if 1 disk dies?** Yes (as far as I know).
- **[24](http://everythingsysadmin.com/the-test.html#netn1). Is the network core N+1?**<span style="color: #ff0000;"> Unknown.</span>
- **[\*25](http://everythingsysadmin.com/the-test.html#backups). Are your backups automated?**<span style="color: #ff0000;"> Unknown.</span>
- **[\*26](http://everythingsysadmin.com/the-test.html#gameday). Are your disaster recovery plans tested periodically?**<span style="color: #ff0000;"> Never to my knowledge.</span>
- **[27](http://everythingsysadmin.com/the-test.html#hydra). Do machines in your data center have remote power / console access?** Yes, HP ILO.

14. [G](http://everythingsysadmin.com/the-test.html#subG). Security practices:
- **[\*28](http://everythingsysadmin.com/the-test.html#malware). Do desktops/laptops/servers run self-updating, silent, anti-malware software?** <span style="color: #ff0000;">No.</span>
- **[\*29](http://everythingsysadmin.com/the-test.html#secpol). Do you have a written security policy?** <span style="color: #ff0000;">No.</span>
- **[30](http://everythingsysadmin.com/the-test.html#secaud). Do you submit to periodic security audits?** Yes, but they are very rudamentary.
- **[31](http://everythingsysadmin.com/the-test.html#acctdis). Can a user’s account be disabled on all systems in 1 hour?** <span style="color: #ff0000;">No, too many one-off systems.</span>
- **[32](http://everythingsysadmin.com/the-test.html#pwch). Can you change all privileged (root) passwords in 1 hour?**<span style="color: #ff0000;"> No, We can change 90%, but not a handful of oneoffs, which are difficult to identify.</span>

Wow… that was… depressing. 10/32= 31% That I could answer yes with some degree of confidence.