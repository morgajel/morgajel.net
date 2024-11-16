---
author: Jesse Morgan
categories:
- Uncategorized
date: "2010-04-13T10:58:34Z"
guid: http://morgajel.net/?p=691
id: 691
title: I am Disappoint, son
url: /2010/04/13/691
views:
- "108"
---

> SUBJECT: \[#24413540\] Suspension Notice CPU Limits Reached (1st Warning)  
> Hello,
> 
> Due to the amount of CPU and/or memory resources used by your account, we were forced into suspending the account. Please understand that we only suspend an account as a last resort and want to help you track down the cause as quickly as possible.
> 
> Shared servers (MegaPhase, Prophase, and ANHosting accounts) are only allotted 10% of the server’s resources (CPU and Memory) at any given time. This  
> restriction is put in place to keep balance on the server. The first step is to  
> try to isolate what content on your website is causing the overage. If this  
> seems unexpected, it could be a recent change to your site (such as installing  
> a new wordpress plugin, mailing list application, etc). Any information you can  
> provide about recent changes can help us to isolate the problem. However, we  
> were able to retrieve the following files and/or scripts that likely caused the  
> suspension:
> 
> ————————
> 
>  literaryescapism.com 2.33 1.38 0.1  
> Top Process %CPU 34.0 \[php\]  
> Top Process %CPU 25.0 public\_html/index.php  
> Top Process %CPU 23.0 public\_html/index.php
> 
> ————————

This was the beginning of an email I received from our hosting company, telling me that Jackie’s site was suspended due to usage. I’m not disappointed so much that they suspended it (which was the proper course of action), but in how they responded to me. Here’s my response to the warning email:

> I’m trying to figure out if this is new behavior from a wordpress exploit or if it’s honestly just grown to this point; do you have any trending data for my cpu usage?
> 
> Side note- the access-logs linked in my home directory appear to go nowhere:  
> access-logs -&gt; /usr/local/apache/domlogs/user  
> that path doesn’t exist.
> 
> I would like to help in any way I can. As a side note, I’ve noticed quite a few “errors connecting to mysql” in the apache logs; could there be an underlying issue with mysql that’s causing processes to back up and chew cpu time?
> 
> I’m also seeing some defunct php processes from other users, leading me to wonder if this is more of a systemic problem than one with just my account.
> 
> (sorry, I’m an apache/linux admin myself- let me know if there’s anything I can do to assist. I can be reached on gtalk, aim or yahoo.)

And their response:

> Hello,
> 
> Unfortunately we do not have any further data except for what was given in the e-mail:
> 
>  literaryescapism.com 2.33 1.38 0.1  
> Top Process %CPU 34.0 \[php\]  
> Top Process %CPU 25.0 public\_html/index.php  
> Top Process %CPU 23.0 public\_html/index.php
> 
> Your account has been unsuspended so you can troubleshoot this issue. Please let us know if you have any further questions.  
> —  
> Best regards,
> 
> xxxxxxxxxxxxxx  
> Technical Support Representative  
> Hosting Services, Inc.

It’s sort of dumbfounding to be completely shut down like that from a TSR. I would have hoped that he would have seen that I A) know what I’m talking about and B) would have booted me up to someone who could better assist me. Instead he unlocks the account (which, btw, was never locked or suspended as far as I could tell from the time I got the email and checked until he sent the notice that it was unsuspended) and says “sorry, we have no data to help you figure out what happened.” Note that he didn’t address my questions regarding the broken symlink, mysql availability, or defunct processes.

Flustered, I responded with this:

> Is it still using such high CPU load? I noticed several processes on the server that were having issues when I looked; many of which were not mine- The load on the server was 114 when I checked it last night. Regardless of server, that’s high, and I wasn’t one of the high processes listed.
> 
> I suspect this was a systemic issue- perhaps mysql went down causing multiple sites to freak out. If that’s the case (and there was a problem with the system itself), troubleshooting from my end is near impossible, especially since you have no trending data on usage. To determine if this was a one-time issue or a building issue.
> 
> I’d like to help you guys, but ya gotta work with me here. This is what I do for a living- I’m the Apache/Linux administrator. My wife’s site has been running for 6 months with no major changes (other than simple wordpress upgrades, none of which  
> were in the last week) and no notable increase in traffic or bandwidth usage over the past week.
> 
> If you’re incapable of providing details as to whether or not there was a system issue (or even if other clients had similar issues at the same time) my only course of action would be to take our sites to another provider. I don’t want to do that; I’ve been happy with you guys so far.
> 
> Please, check and see if there were multiple clients on this server who received warnings as well; I suspect I wasn’t the only one. From where I’m standing the server does not look very healthy- I’m seeing multiple defunct processes:
> 
> nobody 6857 0.0 0.0 0 0 ? Z 08:46 0:00 \[httpd\] <defunct>  
> 1027 10565 15.0 0.0 0 0 ? Z 08:48 0:00 \[php\] </defunct><defunct>  
> 32003 10573 0.0 0.0 0 0 ? Zs 08:48 0:00 \[cpsrvd-ssl\] </defunct><defunct></defunct>
> 
> Please let me know if there is a sysadmin I can work with to resolve this issue. Like I said, I don’t think this is a problem with my site necessarily.

We’ll see how they respond. Hopefully they’ll be useful- I just switched to them in October, so I really don’t want to have to find another host.