---
author: Jesse Morgan
categories:
- administration
- Hobbies
- Work
date: "2010-06-30T22:01:45Z"
excerpt: As a system administrator, monitoring is a key job responsibility, yet arguments
  seem to arise on how to implement it (usually with people who won't be paged at
  3am). Before writing this, I looked around for an article on the goals and philosophy
  of system monitoring, but found very little that really applied to this topic. Hopefully
  this will help set some expectations for admins, managers and stakeholders on what
  you should monitor, and why it should be monitored.
guid: http://morgajel.net/?p=755
id: 755
tags:
- linkedin
title: The Philosophy of Monitoring
url: /2010/06/30/755
views:
- "325"
---

As a system administrator, monitoring is a key job responsibility, yet arguments seem to arise on how to implement it (usually with people who won’t be paged at 3am). Before writing this, I looked around for an article on the goals and philosophy of system monitoring, but found very little that really applied to this topic. Hopefully this will help set some expectations for admins, managers and stakeholders on what you should monitor, and why it should be monitored.

## Why you Monitor

Before you set up a single monitor, you have to ask yourself, “*what is the goal*?” After all, why are you even setting something up? Here are a few common reasons for configuring monitors:

1. **Notification:** Warning of an issue that requires intervention. What most people think of when you say “Monitoring”.
2. **Reactionary:** Automatic actions are taken when certain criteria are met. If common countermeasures are automated, you’ll have less to handle manually.
3. **Informational:** System status and historical trending allows you to show business customers that production “isn’t always down.” In reality, you may have 99% uptime, and often downtime is due to requested deployments. Statistical information can also be used for capacity planning.

Mentally dividing your monitors into groups will help you calculate which monitors require involvement. It’s not uncommon to have several thousand monitors at any given time, so it’s important not to assign critical importance to all of them. A wise man once said “When all alerts are critical, none of them are.”

## When you should NOT Notify

Some monitors may have thresholds set which check for certain conditions; when those conditions are met, you may want to send some type of alert to an administrator. There are two types of notifications – Active and Passive:

- **Active Notification:** Immediate Action is Required: “Site is Down!” A phone call, page, or IM may be used to contact someone. Direct action expected.
- **Passive Notification:** Informational Purposes only: “JVM Memory usage is high.” Information is logged, and perhaps an email is sent. No direct action is expected, since there’s usually nothing you can do about it.

It’s easy to become addicted to passive notifications – but remember, data overload can mask important information. It becomes habit to ignore notifications if they are unimportant. The question then is not so much “when should you notify,” but “when shouldn’t you?” What it really boils down to is “Can/should I do anything about it right now?”

- Non-critical (disk space creeps above 90% on /var on a dev server at 2am on a Saturday after several months of growth).
- Nothing Systemic is wrong (admins can’t fix “low sales”).
- 3rd party system, such as a geocoding webservice, is down.
- Will resolve shortly, such as a backup server pegging the CPU during midnight backups.

Some of these alerts can be avoided by setting a correct monitoring window (ignore CPU during the backup window, or set a blackout window for a deployment). Others simply can’t be addressed by administrators, although you may want to send informational emails to other members of the company (those managing 3rd party SLAs or responsible for tracking online sales)Â The next step after getting an alert is figure out what to do about it.

## Reacting Properly

When a notification is sent out, there should be a definitive action that you can take. Think about why you were notified. There are a few rules to keep in mind when something goes wrong.

1. **Don’t Panic.**  When 700 alarms go off, your first instinct is to panic. Before you act, take a breath. Spend a moment to get your bearings, and calm yourself. The worst possible thing you can do is flail. Randomly making changes without rhyme or reason and restarting services can do more harm than good and may make the situation worse. Take note of which alarms go off, and in the post mortem look for ways to get the same information with less noise.
2. **Identify Obvious Patterns.**  What is the commonality? If a central system goes down, you may see many similar alerts. Dependencies can help immensely, masking redundant alerts. A single database failure could take down a dozen sites. Which is better: getting a single alert that the database is down, or 250 alerts that various sites are down and one database notification in the middle? While 250 alerts may impress the gravity of the situation upon you, it may instil panic and anxiety, which leads to flailing.
3. **Get things up and running as quickly as possible.**  Root-cause analysis can be tedious, time consuming, and occasionally inconclusive. If you have a major system outage, don’t worry about doing root-cause analysis on the spot. Do what you need to in order to get things up and running – you can search the logs later. If the problem is recurring, you’ll get another chance to investigate later.
4. **Communicate with Stakeholders.**  The business units don’t need to know the details, but they do need to know that there is an outage and that it’s being addressed. If the situation is not quickly resolved, give them status reports. Be warned – any details you reveal will be warped and held against you. I’ve learned this one many times. People have a tendency to blame what they don’t understand. “Site is down? It must be a witch!” At a previous job we had a “jump to conclusions” board which had our favorite scapegoats – load balancer, connection pool, Endeca, etc. Everyone is guilty of it – Business, devs, sysops, QA, etc. Even a one-time problem that has been resolved will be brought back up, even if it’s only tangentially related. Communicating too much information creates future scapegoats.
5. **Contact Domain Experts.**  If your java site is crashing and you’re not a java developer, get a java developer involved. If your DNS server falls down and the fix isn’t obvious, contact your DNS administrator. Expert eyes on the problem may resolve the issue quicker. Group chat is crucial for sharing information and talking out theories. Someone familiar with the code will know what the error messages mean.
6. **Fix the Problem.**  It should go without saying that if you find the problem, you should make every effort to resolve it. Workarounds are fine, just don’t let that band-aid become permanent. What often happens is a workaround is put in place; the alert clears and management no longer feels the pain, so they ignore the problem without putting forth the effort to fix the issue. When the next issue appears, a new fix is layered on the old. Band-aid is layered on band-aid. Eventually you’ll need to pull those band-aids off; and the more there are, the more painful it will be.

## How Much is Too Much?

Most administrators prefer to be proactive rather than reactive, resolving issues before they become a problem. Proper monitoring can be a great asset, but if you’re not careful it can cause problems. For example, at a previous job we had a load balancer, apache instances and tomcat instances set up for each site. Each site had the following:

**In (Sitescope) legacy monitoring system:**

- Health check on load balancer URL

**In Nagios:**

- Health check on Apache instances
- Health check on Tomcat instances
- Health check on Load balancer URL

**In Apache:**

- Health check on tomcat instances

**In Load balancer:**

- Health check on Apache instances
- Health check on Tomcat instances

Individually, these don’t seem that bad. If an apache instance goes down \*of course\* the load balancer needs to know so it won’t send traffic to that instance. The same with Apache watching Tomcat. The problem was the frequency of the checks; the load balancer was checking each monitor every five seconds. When a poorly load-tested site update was released, certain pages took 7 seconds to load. Things quickly went downhill as threads and processes backed up, crashing the site.

Balancing responsiveness with common sense is essential. Having a monitor check every minute won’t change the fact that it will take an admin 20 minutes to get to a computer, boot up, log into the VPN, and identify the issue. Don’t add to the problem by DOS’ing your applications.

## Making Contact

One mistake I’ve seen is using email as a reliable and immediate method of contact, often expecting a quick response. My favorite is when someone sends you and email, then walks down to your desk immediately after and asks “did you see my email?” You check and see it was sent literally less than two minutes ago. You can’t rely on people to continually check their email. Admins especially don’t due to the sheer volume we receive.

Email has it’s uses, but active contact in an emergency situation is not one of them. Personally, I only check my email when I think about it, which may mean large delays between when the message is sent and received. Couple that with spam filters, firewalls, solar flares and the 500 other unread messages and email becomes a less-than-reliable medium for emergency notifications (even during business hours).

Paging (or SMS) is preferable if you expect a quick response, although it is far from perfect. Just like email, SMS messages can be lost in the ether, however recipients usually have their phone alert them when a message comes in since it happens far less often than an email drops into the inbox. That said, every alert should not be sent as a page, or apathy will quickly sink in. The escalation path should look something like this (although all steps are not needed):

- **Front-end web interface alert:** User would have to actively be browsing to see the status change. Usually the first clue something is wrong and shows the most recent status changes on a dashboard.
- **Email Alert:** User would have to be actively checking their email. Usually sent when something is first confirmed down.
- **Instant Message:** User would have to be at a computer and logged into IM to receive the alert. Rarely used, but an option during business hours.
- **Page/SMS:** Reserved for emergencies. This means there is trouble.
- **Phonecall:** Only used if Admin does not respond to the previous contact attempts. Usually performed by an irate manager or director.

If you’re lucky enough to have a 24×7 call center / help desk, they can also be leveraged to resolve issues before a system administrator is needed. If recurring patterns start to emerge, automation can be used to deal with the problem (or better yet you can fix the underlying issue). Sadly, many issues can’t be automated away or solved by a call-center staffer pressing a button. A real admin will eventually need to be contacted.

I don’t want to dig too deeply into on-call rotations, but an effort should be made to balance off-hours support with a personal life. Being on-call means no theaters, fancy dinners, or quality time with the family. Without balance, burn out will ensue.

## Afflictions

System monitoring often brings out odd behavior in even the most steadfast of administrators. Some behaviors are relatively benign, while others can cause severe problems down the road. Identifying these behaviors before they cause a problem is just as important as having good monitors.

- **Data Addiction:**  Knowledge is power, but do not mistake information with knowledge. It’s possible to have 700 alerts, and not one of them identify the underlying issue. One of my least favorite phrases is “Can we put a monitor on that?” It’s often uttered right after a one-off failure; the type of thing that fails once, and once fixed will never cause a problem again. An example of this is a new server, where apache was not configured to restart after a reboot. When the server is restarted, you quickly find apache is down, start it, configure it to auto-start, and move on. There is already a monitor on the websites hosted by that apache instance as well as a monitor on how many apache threads are currently running; What purpose would another monitor serve? How often would it run? This is a prime example of how a data addict can spin out of control – too many useless monitors will mask a more important issue.
- **Over Automation:**  Automation is a wonderful thing, however, it’s possible to have too much of a good thing. In one instance, there was a coldfusion server which would crash often. Rather than trace out the root cause, restarts were automated, then forgotten about. A few years later, it was found that the coldfusion servers were restarting every twenty minutes, and no one knew about it – no one except the users. If it takes 20 seconds to restart, and that’s 26280 twenty-second interrupts over the course of a year – that can translate into a bad user experience and loss of sales. Make sure that automation is audited and verifiable, and doesn’t cause more trouble than it prevents.
- **Over Communication:**  While it is important to communicate with stakeholders, it is possible to over communicate. Stakeholders don’t need to know that there are 130 defunct apache processes caused by a combination of a bug in mod\_jk and the threading configuration in JBoss – all they need is “Site availability is intermittent – we’ve located the root cause and are working on a solution. More information to follow.” Details aren’t needed. Likewise, not every single person should be notified when an alert goes off – does your backup administrator need to know when a web server goes down? No. Does the DBA need to know when an SSL cert is about to expire? No. Tailor the messages to the correct audience. Most monitoring systems allow you to configure contact groups – use them.
- **Complexification:**  There are dozens of relationships between services, hosts, hostgroups, contacts, servicegroups, notification windows, dependencies, parents, etc. Try as you might, it’s usually impossible to perfectly model every relationship. Don’t become distracted by perfecting the configuration – focus on maintainability, scalability and accuracy. If you can’t add new systems and monitors, your configuration is too complex.
- **Reporting vs Monitoring:**  Reports are the more successful cousin of Alerts. They may superficially appear similar, but serve entirely different purposes. Monitors should only be used to track and trend data and to notify if there is a problem, whereas reports take the collected data and massage it into an aggregated format. Monitors shouldn’t send out scheduled alerts. They can collect data, but they shouldn’t be used to present it to users. You’d be surprised how often someone asks for a monitor to send a nightly report. That slippery slope will turn your monitoring system into crystal reports.
- **False Positives:**  False positives are the scourge of the monitoring world. There are many causes, but the reaction is always the same – start to investigate, realize that it’s a false positive, and lose interest, knowing that nothing is broken. The problem is that a false positive leads to lazy behavior – if you’re pretty sure it’s a false positive, you don’t bother looking into it, figuring it will clear on it’s own. This trains people to have a “wait and see” mentality when alerts go off, causing unneeded delays when a major issue appears.
- **Apathy:**  It’s 2am on a Saturday, and you get paged that the CPU on a utility server is pegged. Without looking, you know that it’s the backup process copying the home directories, so you ignore it. The following Monday at 10am the QA JBoss instance stops responding. You know that it will clear within minutes because the QA team always rebuilds the QA instance Monday morning. When you get monitors constantly failing and recovering on their own, you start to ignore the pages that come in because you know they’re unimportant. It’s only a matter of time before you miss something important. If you have a situation that promotes apathy towards alerts, resolve it before something important is missed.

## Don’t be \[A\]pathetic

I mentioned apathy above, but there’s a bit more to it – it’s not just admins that become apathetic. If an issue is identified, action must be taken to correct it. The coldfusion example mentioned above is a great example of company apathy – failure of the business unit to prioritize it and failure of IT to push back hard enough. A former manager once had someone laugh because his team had ignored my manager’s bug report for a full year. That’s not funny; it’s pathetic.

When management fails to address an issue; be it a known system problem or something as simple as morale from a lost team member, it shows the team that they don’t care. It soon becomes a vicious cycle of uncaring when managers no longer care that the site is down, which in turn causes developer apathy. Developers then don’t care about code quality, leading to buggy code. Sysops stop caring that alerts are going off, leading to downtime. By the time the cycle is broken, it’s far too late – you’ve established a bad reputation with your customers.

Often times this will start with unreasonable development expectations, causing devs to cut corners, QA to be rushed, and monitors to be forgotten. There is a balance that must be maintained between getting code out the door and making sure that the code can stand up to the abuse it will receive when it goes live. It’s a team effort, and everyone must care (and keep caring) to keep the systems running.

Wow. Well, that’s a lot more than I intended on writing. I should state that I am guilty of 75% or more of the bad behaviors listed here. I hope that this will help start discussion on how to better improve monitoring systems.

If you have feedback, suggestions or enhancements, please leave them in the comments.

(Thanks to jdrost, jslauter, keith4, pakrat, romaink, and my wife Jackie for their peer review/editing.)