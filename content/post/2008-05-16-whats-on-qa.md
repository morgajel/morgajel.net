---
author: Jesse Morgan
categories:
- Entertainment
- IRC
- Work
date: "2008-05-16T10:44:49Z"
guid: http://morgajel.net/2008/05/16/246/
id: 246
title: What&#8217;s on QA&#8230;
url: /2008/05/16/246
views:
- "86"
---

Had an amusing conversation with a developer at work that had the feeling of an abbot and costello bit. I’m leaving his name out of this to protect him, but he’s read this site before and will know instantly that it’s him. The conversation revolves around our new continuous integration system, and how the terminology has changed.

BTW, QA=Quality Assurance, UAT= User Acceptance Testing (staging)

<font color="#204a87"></font><font size="2">(10:50:50 AM) </font>**morgajel:** ok, so after talking to mick, it looks like my suspicions were correct  
<font color="#204a87"></font><font size="2">(10:50:54 AM) </font>**morgajel:** there is no QA  
<font color="#204a87"></font><font size="2">(10:51:04 AM) </font>**morgajel:** the name QA is misleading.  
<font size="2"></font><font color="#cc0000">(10:51:16 AM) </font><font color="#cc0000">**DeveloperX:**</font> hmmm  
<font color="#204a87"></font><font size="2">(10:51:25 AM) </font>**morgajel:** trunk is for development, release is for qa, uat and prod  
<font size="2"></font><font color="#cc0000">(10:51:26 AM) </font><font color="#cc0000">**DeveloperX:**</font> what do u mean  
<font size="2"></font><font color="#cc0000">(10:52:47 AM) </font><font color="#cc0000">**DeveloperX:**</font> i have a release branch…  
<font color="#204a87"></font><font size="2">(10:52:50 AM) </font>**morgajel:** think about it- anything going to QA is being marked as a release.  
<font color="#204a87"></font><font size="2">(10:53:12 AM) </font>**morgajel:** so so release gets deployed to QA, then if it passes it goes on up the chain to production  
<font size="2"></font><font color="#cc0000">(10:53:52 AM) </font><font color="#cc0000">**DeveloperX:**</font> this is how the system currentlyis  
<font size="2"></font><font color="#cc0000">(10:54:23 AM) </font><font color="#cc0000">**DeveloperX:**</font> it seems to make snese to me? assueming nothing makes it ti UAT unlees it goes through QA  
<font color="#204a87"></font><font size="2">(10:54:36 AM) </font>**morgajel:** that’s the way it should be.  
<font size="2"></font><font color="#cc0000">(10:54:49 AM) </font><font color="#cc0000">**DeveloperX:**</font> i think thats how it is  
<font color="#204a87"></font><font size="2">(10:54:57 AM) </font>**morgajel:** the difference between this and the old system is, rather than labelling it projectX\[QA\], they’re now calling projectX\_release\_  
<font size="2"></font><font color="#cc0000">(10:55:07 AM) </font><font color="#cc0000">**DeveloperX:**</font> no? i only have a trunk and a release  
<font color="#204a87"></font><font size="2">(10:55:11 AM) </font>**morgajel:** that’s the main difference for you  
<font size="2"></font><font color="#cc0000">(10:55:20 AM) </font><font color="#cc0000">**DeveloperX:**</font> which means the only thing goingto UAT will be from the release  
<font color="#204a87"></font><font size="2">(10:55:51 AM) </font>**morgajel:** correct- the only thing going to qa or uat or prod will be from the release.  
<font size="2"></font><font color="#cc0000">(10:56:12 AM) </font><font color="#cc0000">**DeveloperX:**</font> so why can’t we still call it QA?  
<font color="#204a87"></font><font size="2">(10:56:42 AM) </font>**morgajel:** because it’s \*more\* that QA- QA was a bad name for it from a organizational perspective  
<font size="2"></font><font color="#cc0000">(10:57:32 AM) </font><font color="#cc0000">**DeveloperX:**</font> is his only going to affect me, or is this the new approach for everyone?  
<font color="#204a87"></font><font size="2">(10:57:40 AM) </font>**morgajel:** for everyone  
<font size="2"></font><font color="#cc0000">(10:58:27 AM) </font><font color="#cc0000">**DeveloperX:**</font> so your saying there is no pointin multiple srevers?  
<font color="#204a87"></font><font size="2">(10:58:33 AM) </font>**morgajel:** no  
<font color="#204a87"></font><font size="2">(10:59:04 AM) </font>**morgajel:** I’m saying whatever we put on UAT should be the EXACT same thing as QA  
<font size="2"></font><font color="#cc0000">(10:59:13 AM) </font><font color="#cc0000">**DeveloperX:**</font> so we will still push from trunkto release to UAT?  
<font color="#204a87"></font><font size="2">(10:59:20 AM) </font>**morgajel:** and whatever we put in prod should be the EXACT same thing as both of them.  
<font size="2"></font><font color="#cc0000">(10:59:24 AM) </font><font color="#cc0000">**DeveloperX:**</font> it is  
<font size="2"></font><font color="#cc0000">(10:59:28 AM) </font><font color="#cc0000">**DeveloperX:**</font> currenlty  
<font color="#204a87"></font><font size="2">(10:59:29 AM) </font>**morgajel:** well, from your side, here’s what happens  
<font size="2"></font><font color="#cc0000">(10:59:37 AM) </font><font color="#cc0000">**DeveloperX:**</font> at least for anything i;ve worked on  
<font color="#204a87"></font><font size="2">(10:59:44 AM) </font>**morgajel:** you guys develop in trunk, and when you’re ready to release, you mark it as release.  
<font size="2"></font><font color="#cc0000">(10:59:53 AM) </font><font color="#cc0000">**DeveloperX:**</font> k  
<font color="#204a87"></font><font size="2">(10:59:55 AM) </font>**morgajel:** then the QA people will check out release to the QA servers  
<font size="2"></font><font color="#cc0000">(11:00:04 AM) </font><font color="#cc0000">**DeveloperX:**</font> k  
<font color="#204a87"></font><font size="2">(11:00:10 AM) </font>**morgajel:** once they give it a go, we will check it out to UAT,   
<font size="2"></font><font color="#cc0000">(11:00:22 AM) </font><font color="#cc0000">**DeveloperX:**</font> k  
<font color="#204a87"></font><font size="2">(11:00:24 AM) </font>**morgajel:** once that’s good, it will be checked out from release toprod,   
<font size="2"></font><font color="#cc0000">(11:00:54 AM) </font><font color="#cc0000">**DeveloperX:**</font> put the code that is working it’s way up is exacly the same in each instance  
<font color="#204a87"></font><font size="2">(11:01:12 AM) </font>**morgajel:** correct- as it should be.  
<font size="2"></font><font color="#cc0000">(11:01:18 AM) </font><font color="#cc0000">**DeveloperX:**</font> it is jsut being push multiple times, to make sure everything will work when we finally hit orid  
<font size="2"></font><font color="#cc0000">(11:01:20 AM) </font><font color="#cc0000">**DeveloperX:**</font> prod  
<font color="#204a87"></font><font size="2">(11:01:27 AM) </font>**morgajel:** correct  
<font color="#204a87"></font><font size="2">(11:02:06 AM) </font>**morgajel:**  
now, I’m not sure what the exact mechanism will be for pushing to uat  
and prod- we may check it out directly from svn, but more likely than  
not, we won’t.  
<font size="2"></font><font color="#cc0000">(11:02:33 AM) </font><font color="#cc0000">**DeveloperX:**</font> ya, i would guess not  
<font color="#204a87"></font><font size="2">(11:02:37 AM) </font>**morgajel:** the important thing is we can say “release 11234234 of projectX” and can match it up with a specific commit in subversion.  
<font size="2"></font><font color="#cc0000">(11:03:50 AM) </font><font color="#cc0000">**DeveloperX:**</font> i gotcha there…we used to havesomethiing like that, but it completely maintianed by me  
<font size="2"></font><font color="#cc0000">(11:04:16 AM) </font><font color="#cc0000">**DeveloperX:**</font> i have a new release branch for every deployment i make to projectX  
<font size="2"></font><font color="#cc0000">(11:04:34 AM) </font><font color="#cc0000">**DeveloperX:**</font> so I code in theory go back to 3deployments ago  
<font size="2"></font><font color="#cc0000">(11:04:46 AM) </font><font color="#cc0000">**DeveloperX:**</font> but you guys should have access to this…not just me  
<font size="2"></font><font color="#cc0000">(11:05:35 AM) </font><font color="#cc0000">**DeveloperX:**</font> make sense?  
<font color="#204a87"></font><font size="2">(11:08:09 AM) </font>**morgajel:** you’re close  
<font color="#204a87"></font><font size="2">(11:08:23 AM) </font>**morgajel:** I’m presuming you’re talking about http://buildserver/svn/projectX/branches/, correct?  
<font color="#204a87"></font><font size="2">(11:08:32 AM) </font>**morgajel:** each of those listed is a different release branch, correct?  
<font color="#204a87"></font><font size="2">(11:09:01 AM) </font>**morgajel:** what they’re doing is instead of creating a folder for each release, they’re creating a single folder  
<font color="#204a87"></font><font size="2">(11:09:17 AM) </font>**morgajel:** and making note of the commit version when they commit acode to it  
<font color="#204a87"></font><font size="2">(11:10:14 AM) </font>**morgajel:** hence when subversion says “committed version #1123223”,you’d call it “release 1123223” in your notes  
<font color="#204a87"></font><font size="2">(11:10:30 AM) </font>**morgajel:** I think that’s what brandon is trying to aim for, but we’ll have to have him verify.  
<font size="2"></font><font color="#cc0000">(11:11:00 AM) </font><font color="#cc0000">**DeveloperX:**</font> ok, i think i gocha