---
author: Jesse Morgan
categories:
- Development
- SPoE
date: "2010-08-21T21:13:29Z"
guid: http://morgajel.net/?p=925
id: 925
tags:
- agile
- Card
- SPoE
- Stories
- User
title: 'SPoE: Slow but steady.'
url: /2010/08/21/925
views:
- "77"
---

So I have [a few user stories](http://morgajel.net/2010/08/17/908); time to start putting the infrastructure together. So What have I decided on so far?

**Language:** Java  
**Framework:** Spring[\*](#spring)  
**Repository:** Subversion  
**IDE**: Eclipse  
**Continuous Integration/ Deployer**: Hudson  
**Build Automation**: Maven2

I’m in the process of getting all my pieces together and in place. I’ve set up a [subversion repository](https://www.morgajel.com/svn/spoe/trunk/) and eclipse. I have a very basic .war file setup committed and a maven script to build the war file. Every five minutes, a [Hudson](http://boron.morgajel.com/hudson/) job polls the repository and rebuilds the warfile, then [deploys it to JBoss](http://spoe.morgajel.com/index.jsp). It’s a pretty sweet setup despite it only deploying a “Hello World”.

Now I need to refresh myself on Java and learn Spring.Â This leads me to my first task:

> **Card SPoE-0001**   
> **User Creation**
> 
> As a User, I want to be able to provide a preferred username and email address to create an account.
> 
> **Status:** New  
> **Version:** 0.0.1  
> **Component:** User account  
> **Original Estimate:** ?  
> **Time spent:** 0  
> **Time Needed:** ?

Going into this, I’m familiar with MVC framework in Rails, so I’m guessing the concepts aren’t much different for Spring MVC I’m still new to this process(Java webapp layout, Agile and Spring), so if you see any mistakes or have suggestions, please let me know. If you’re interested in helping me with development,

<a name="spring">\*</a>Despite suggestions of both Struts and Spring, I went with Spring after [scientific investigation](http://www.googlefight.com/index.php?lang=en_GB&word1=java+struts&word2=java+spring).