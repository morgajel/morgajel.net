---
author: Jesse Morgan
categories:
- administration
- Hobbies
date: "2011-12-12T01:03:41Z"
guid: http://morgajel.net/?p=1108
id: 1108
tags:
- linkedin
title: Using Jenkins for System Administration
url: /2011/12/12/1108
views:
- "944"
---

# Preface

While system administrators often have many different goals, here are two that seem fairly universal:

- Automate the redundant tasks
- Hand off the simple tasks

I’ve recently found that the build utility Jenkins can be a major boon for an Operations team, and wanted to share my findings with others.

## What is Jenkins?

So, what exactly is Jenkins? It’s a popular fork of the open source continuous integration tool, Hudson. While it is normally used for building and deploying software, it can easily be used for more interesting purposes. Here are some of the more useful features:

- LDAP authentication
- Group/Matrix-style permissions
- User-definable views
- Console output for each run
- Manual and/or cron-based runs
- master/slave node configurations
- Email, XMPP, IRC notification integration
- integration with Bash, Ant, and Maven
- integration with cvs, subversion and other version control systems
- Dozens of useful plugins.

Due to it’s modular and flexible nature, not only can you pick and choose the plugins you want, but you can write your own as well.

## What is Jenkins normally used for?

Jenkins’ intended use is as a Continuous Integration server- It downloads code from a repository, resolves dependencies, builds the code, and then deploys it. As mentioned before, it tightly integrates with both Ant and Maven, making it a boon for java developers. While Ant and Maven integration is incredibly useful, it’s the combination of subversion and bash integration that sysadmins will find useful.

## What can Jenkins be used for?

Don’t think of Jenkins as a continuous integration server, think of it as…

- Cron with a Gui Interface and nice bells and whistles
- Sudo replacement
- Documentation updater

Jenkins has the advantage of centralization, visibility (a pretty gui goes a long way), per-run logging, IM and/or email notification, time trending, and a self-contained workspace. I usually draw the line by seeing if one or more of the following statements apply:

- Use Jenkins if it involves multiple hosts
- Use Jenkins if you wish to capture output on each run
- Use Jenkins if you wish to trend disk usage or time of the task
- Use Jenkins if you are in need of ACLs
- Use Jenkins if it allows a non-technical person to perform a task without the help of a sysadmin
- Use Jenkins if you want to run something manually as well as on a schedule.

## What shouldn’t Jenkins be used for?

Once you recognize the power and flexibility at your disposal, you may be tempted to use Jenkins as a

- Cron with a Gui Interface and nice bells and whistles
- Sudo replacement
- Documentation updater

See what I did there? My point is that there’s a fine line between what belongs in Jenkins and what does not. Discovering it for yourself is half the battle. Here are some sample tasks as I would categorize them.

| Task | Sudo | Cron | Jenkins |
|---|---|---|---|
| Log rotation on a single server |  | X |  |
| Login user access to restart an init script | X |  |  |
| Multi-user access to restart an stop an init script, clear logs and restart an init script |  |  | X |
| Manager access to restart a service |  |  | X |
| Deploying code across several production nodes |  |  | X |
| Update Documentation on a Given Project |  |  | X |
| Run a command nightly | X |  |  |
| Run a command nightly that emails it’s output | X |  |  |
| Run a command manually that IM’s you when complete and logs STDOUT |  |  | X |
| Run a command through PDSH across several servers and update a wiki page | X |  | X |

## Why not Mcollective or RunDeck?

To be honest I’d never heard of them until after I began writing this article. I’d already been using Jenkins/Hudson for a year and a half. There may be better tools for the job, but Jenkins has some distinct advantages.

1. You may already have it
2. Getting buy-in from developers to use it is much easier.

In either case many of the tasks below may be usable under work-alike services.

# Sample uses of Jenkins

So lets get down to business- how can Jenkins make your life easier? Your first task is to identify things that can easily be scripted, are incredibly repetitive, and produce data that is outdated quickly. For me, that first task was to generate documentation.

## Generating Documentation

No documentation is better than scores of bad documentation- It’s better to know you don’t know, than to know the wrong thing. One of the many problems we suffered from was simple IP Address allocation. Since we didn’t own the DNS server, it was hard for us to track IP address allocation- i.e. we didn’t know what IP addresses were in use, or what they were used for, or who was using them. Our best attempt to track them was manual modification of our team’s 47 internal subnets. The problems with this method were threefold- 1) it involved a human performing an error-prone routine, 2) There was no simple way to audit the entire system, and 3). There was no central authority.

Needless to say, things would drop through the cracks. On more than one occasion, we had applications go down because their IP address was double-allocated because half of the team didn’t know the IP allocation pages existed.

### Example 1: Reverse DNS Entry Cleanup

So how did we solve this? The first step was to audit the current status of our reverses. I wrote a perl script that takes IP ranges as an input, scans the network for active IPs, does reverse lookups on all IP addresses, then documents oddities and writes them to the wiki.

This may seem simple, but it was the first time we saw a glimpse at how ugly our reverses were- multiple IP addresses resolving to the same host name (which shouldn’t happen in our configuration), missing reverses, IPs that didn’t respond to ping, but had reverses designated (usually outdated entries). While running this task was simple enough to do manually, it took 30 minutes to run. Who wants to run a 30 minute CLI job? While I could have put it in cron, I chose to put it in Jenkins for the following reason:

- The status was visible; anyone logged into Jenkins could see the process was running, and check the STDOUT to make sure there were no unexpected errors.
- When it completed successfully, the status bulb would turn green- A quick visual sweep of 30 jobs can be done in seconds if you’re just looking for a red bulb.
- Logs were kept with each run, so I could compare and contrast output
- An IM could be sent when the job completed, passing or failing
- the latest version of my perl script was always checked out.
- I could trend run times and estimate how long the next run would take
- I could set it to run automatically once a night at 3am
- I could run it manually via a Jenkins “push button”
- anyone on my team could run it manually via a Jenkins “push button”

With this output, we were able to drastically reduce the volume of bad reverses. With the help of the Confluence.pm module, we could write the results to the wiki for the whole world to see. This however only showed us bad entries- what about the good ones?

###  Example 2: X.X.X.X IP Range Pages

Now that we had reasonably good reverses, we can use that information dynamically generate the pages my coworkers were previously maintaining manually.

We started yet another perl script that took a set of ranges as inputs- each range would generate a different page in the wiki under the name “X.X.X.X IP Range”. The script would then plow through each IP address in the range and

- Do a reverse lookup to find out what host name resolved,
- Fping it to see if it responded,
- Check our LDAP inventory repository (also automatically updated) and report the physical host where the IP was bound,
- Report the primary IP of that physical host,
- The “description” field of the host entry,
- Hardware detail of the host entry.

Once the script was written, it could take any number up IP ranges (even 47 subnets) and rip through them, updating a page for each. Jenkins made sure that the job ran nightly, again pulled the latest version of my script from subversion, and kept track of any oddities in it’s output. Suddenly we had IP allocation tables that were guaranteed to be up to date.

### Example 3: Hardware Breakdown

Inventory management was a weak point for us when I started- we simply didn’t know how many servers we had. Previously this had been managed by a spreadsheet on the hardware admin’s laptop, but much like IP address allocation, it was error-prone and often out of date. We moved to an LDAP-based inventory management setup (How the inventory script works is outside of the scope of this article) that was dynamically updated, however an LDAP interface is less approachable than a spreadsheet for management.

One of the things we track is hardware makes and model numbers. One of the managers wanted to know how many P-class blade we had left- a quick LDAP search and poof, we had his answer. Then he wanted to know how many C-class blades we had, so I showed him how to do searches. While that sufficed for a while, it soon became apparent that we needed a prettier hardware breakdown for management.

One Perl script later, we were able to dynamically generate wiki pages from this data. You may be asking “why not do this with cron?” The answer is simple- dependencies. the Hardware breakdown page is downstream of the Inventory Update script- After all, if it’s pulling the entirety of it’s data from LDAP, it only makes sense to update when that inventory is updated.

### Final note on Generating Documentation

It’s not only the content of the documentation that’s important- my generation scripts also include a warning that the page is auto-generated, the repository location of the script that generated it, and the time it took to generate that specific page.

## Generating Configurations

I’ll be the first to admit that this is a special-case usage, and may not be useful to many, but there are occasions where you’ll want to generate configurations- Not multi-server configurations where Puppet could be useful, but single, complex, repetitive configurations like Nagios.

### Generating Nagios Hosts

We have over 800 servers. Manually configuring Nagios was labor-prohibitive, so an automated approach is warranted. Using our continuously updated inventory, we pull pertinent information from LDAP and generate individual host configuration files for each host. At this stage, there are no checks associated with the hosts.

From there, we loop through our newly created host files and scan the network using a predefined set of “features”- Some hosts run MySQL on port 3306, some run JBoss on 8080, etc. Hosts that match a feature are added to a dynamically generated host group with predefined service checks.

For example, only 2 services should be running on TCP port 8080, Tomcat and JBoss. if that port is active, we check 1161, the SNMP port to determine which service it is, then add it to the proper service group. If it reports as JBoss, the host is added to the JBoss-hosts host group, which has checks for heap usage, thread usage, etc.

So if it’s all automated, why bother with Jenkins? Well, there’s a lot of complexity, and even with the best documentation, it’s a chore for even one person to keep track of how it all works. If something happens to the infrastructure while I’m out (e.g. several servers fail and are replaced), we want to implement those and update monitoring quickly. Having a push button “Regenerate Nagios hosts” makes it simple enough for anyone to do it. Having it email me when it happens, create runtime logs, and pull or write to subversion is icing on the cake. Jenkins helps us ensure that each run is handled identically, and ensures consistency when used by various administrators.

## Empowering Users

Sysadmins usually have an abundant backlog of tasks, be it system audits, upgrade, research, etc. Quite often system administrators get drawn in to user tasks because the task requires elevated privileges. Jenkins can help you hand that off to nontechnical users in a safe and simple manner.

### GUI Interface for Non-Technical Users

No matter how technical your coworkers are, eventually you’ll run into a non-technical user that needs to perform some random technical task; perhaps it’s loading content into a custom system or indexing content. The process may have been designed by a developer using a command line program or script; it may require sudo. In either case, the task is technical enough to make the non-techie shy away from doing it because they “don’t want to use the dos window.”

Jenkins to the rescue! Asking that user to log into a web GUI and press a button lowers the barrier for them. In addition, they can watch the progress, see how long previous runs took, see when the job was last run, and who ran it. If the initial task was in any way complicated, that can all be hidden away (yet still clearly visible in the console logs).

#  Final Thoughts

As you can see, Jenkins can be re-purposed for a plethora of different uses. With a little bit of creativity and ingenuity, you can greatly improve your productivity. If you’re already doing something like this, please leave a comment below describing your own implementations.