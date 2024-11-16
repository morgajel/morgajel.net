---
author: Jesse Morgan
categories:
- Uncategorized
date: "2009-04-26T15:56:37Z"
guid: http://morgajel.net/?p=480
id: 480
title: Jaunty out of the box.
url: /2009/04/26/480
views:
- "386"
---

As some of you know, I enjoy using Kubuntu on my desktop- I have been for the past two years. I’ve been using KDE since it was at 2.1, which was a lifetime ago (note- I remember how awesome 2.2 was in comparison). This past week the newest version of \[K\]ubuntu (9.04- Jaunty Jackalope) was released. Normally I wait a month or two before I try a new release of Kubuntu, but the previous release was so disappointing I decided to skip it. So today, I’ve decided to give Jaunty a try.

###  First impressions

#### Partitioning Issues

I’m not quite sure how, but when I attempted to customize the partition install, I locked up the application so that the edit and delete partition buttons no longer worked. Since there was no way to “back out” of the partitioning at that stage, I was forced to quit. Fortunately, rather than reboot, it dropped me to the live-CD desktop where I was able to rerun the installer and configure the partitioning properly.

#### Network Management Plasmoid

The first thing I do when I set up a freshly installed system is try to get online. KDE4 comes with a nifty networking plasmoid (widget) that looks very slick. Unfortunately I was unable to get it to work with my WPA2 network connection nor my neighbor’s unencrypted connection.

Defeated, I was forced to connected to the network cable in my wife’s office. Once I was online, I downloaded kNetworkManager, the application used in previous releases of Kubuntu to manage internet connections. Although slightly different than what I was using on the older version of Kubuntu I was running on my work laptop, it was easy enough to set up. I was unable to connect to my network (which my work laptop is currently connected to, btw), but I was able to connect to my neighbor’s network. This leads me to believe that there are 2 problems here- WPA2 in general is broke for Kubuntu Jaunty and the Network Management Plasmoid is also FUBAR’d since it can’t connect to a regular unencrypted network.

#### My Broken Alt/ Broken Shortcuts

When I attempted to launch kNetworkManager, I found that the normal Kicker command bar thing wouldn’t pop up when I pressed alt-f2. Nothing. I looked through the system settings for a place to actually set the shortcut to no avail. When I went into **System Settings -&gt; Keyboard &amp; Mouse -&gt; Global Keyboard Shortcuts**, I received the following error message:

> Failed to contact the KDE global shortcuts daemon  
> Message: Could not get owner name of ‘org.kde.kded’: no such name  
> Error: org.freedesktop.DBus.Error.NameHasNoOwner

Once I hit this, the Keybinding configuration was disturbingly blank. Exporting the scheme didn’t throw any errors, but it never created a scheme file for me to examine, so I’m marking the whole thing as a wash.

A while later I noticed alt-tab wasn’t working either, so I figured perhaps it’s a matter of the alt key not being mapped or a wrong keyboard menu. After switching off the god-awful kickoff-style Kmenu (similar to the obnoxious winXP jumbo menu), I went fishing some more under **Settings -&gt; Regional &amp; Language -&gt; Keyboard Layout**, but found nothing of use. I did find however that alt-Left did make the page turn backwards, but I think that’s an application-level shortcut, not a global level shortcut. This means it’s not just a matter of the Alt key being physically broken.

#### Garbled display

I found… for lack of a better work, display corruption whenever I moused over buttons, tabs, boxes, etc in many of the system settings areas. Not sure what caused it, I just know that it’s easily reproducible.  
[![](http://farm4.static.flickr.com/3657/3476137015_a52089171d.jpg?v=0)](http://farm4.static.flickr.com/3657/3476137015_a52089171d.jpg?v=0)

#### Hardware Management

It’s worth noting that this is an IBM Thinkpad T43, and both video and wireless are intel chipsets. The hardware management utility that Kubuntu comes with told me there were no proprietary drivers I could use, so I’m presuming that the video and network issues aren’t caused by those issues. This machine has also run both Kubuntu 8.10 (which really sucked, but this stuff work, I thought) and Ubuntu 8.10 (I don’t like gnome, but at least it worked.)

#### General Kde Crashes

Although it hasn’t done it while I’ve been working on the article, I’ve had various KDE application fail while I’m been playing with it. I’d say maybe 5 crashes in the past 24 hours. Not really acceptable coming from KDE 3.5 where I’d get one maybe once a month.

### Final thoughts

As the battery runs down on this machine while I’m connected to my neighbor’s unsecured wireless, I reflect back on how disappointing this release of Kubuntu has been. For a second release in a row, I’ve been unable to get to to work satisfactorily. The last release I was unimpressed by how inflexible widget placement was on sidepanels as well as missing the Quicklaunch widget I used so often. I didn’t even get that far this release.

Should I give up on Kubuntu? KDE in general? Perhaps I should switch to an RPM-based system.

Here are my options:

1. Wait a few weeks/months and try again.
2. Switch to another KDE-based distribution.
3. Switch to Ubuntu proper or another gnome-based distribution.

Not sure where I’ll end up, but I think I’ll burn a copy of Ubuntu Jaunty and see if I can stomach gnome.