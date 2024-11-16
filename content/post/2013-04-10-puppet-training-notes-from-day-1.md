---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-04-10T07:59:22Z"
guid: http://morgajel.net/?p=1384
id: 1384
title: Puppet Training Notes from Day 1.
url: /2013/04/10/1384
---

I am currently in a training class for Puppet, which is a configuration management tool that I use at work. Yesterday was the first day, and here were the things I’ve learned so far:

1. Puppet can actually create a dependency graph to visualize class relationships. By enabling “graph” in one of the configs, it will produce a .dot file for each host showing their dependency tree.
2. Facter can almost single-handedly replace my linux\_inventory.sh script, and do it more consistently and flexibly. The output is yaml, but it’s easy to translate to LDIF.
3. Using “environments” is far easier than I thought and something I can implement almost immediately. This can feed off the priority number in our hostnames.
4. I can set up puppet-dashboard to be an external node classifier, meaning I no longer need a massive, ugly site.pp file.
5. Puppet-lint can be used to test puppet syntax not only for correctness, but for best-practice formatting. I need to add this to my workflow.
6. The external\_node mentioned in the terminus config line is a script, and I can manually use it to see which classes are applied to which hosts.
7. My current dashboard is ungodly slow, so I need to re-implement it with a proper container.
8. The Enterprise version of puppet can handle certificate signing right in the interface, which is pretty handy and way more user-friendly.
9. The puppet cert –clean command is the proper way to remove an old cert for a host that has been re-imaged.
10. Puppet can integrate with Splunk to feed it its reports, allowing you to correlate puppet events with Splunk events.
11. host resource can get rid of ugly hosts file manipulation.
12. You can set user resource to remove all non-system accounts to ensure no sneaky backdoor accounts are set up.
13. Schedule can be used to run tasks if the run happens in the specified timeframe (Edit: thanks for the better description zipkid).
14. Exported resources can be used to export data, which can then be imported into Icinga, even further reducing the need for “dynamic detection” on Icinga configuration generation scripts.

So as you can see, a lot of good stuff.