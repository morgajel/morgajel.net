---
author: Jesse Morgan
categories:
- Projects
- SPoE
date: "2010-12-11T12:04:03Z"
guid: http://morgajel.net/?p=1004
id: 1004
title: 'SPoE: Loadtesting, Emails and Connections'
url: /2010/12/11/1004
views:
- "86"
---

So as I mentioned earlier, loadtesting with 70 threads failed because the default MTA on Ubuntu (exim) is limited by default to 20 concurrent connections (smtp\_accept\_max). Since I was using under 20% of the cpu and the bottleneck was an unconfigured mailserver, I really wanted to see where my app bottomed out, meaning I needed to fix the mail issue and keep moving.

I should also mention that I switched to localhost to avoid getting myself in trouble by sending out 10,000 registration emails (when I checked the exim queue, there were 28.5k unsent email- oops).

I spent a couple hours trying to configure Exim with a higher smtp\_accept\_max, but I couldn’t figure out how to get the split configuration file to read smtp\_accept\_max, regardless of which of the 30 config fragments I tried placing it in. I also did a dpkg-reconfigure to switch it to the monolithic configuration, and it seemed to accept smtp\_accept\_max (according to exim -bP smtp\_accept\_max smtp\_accept\_max\_per\_host), however the RCPT was refused, so it was a non-starter. I fought for a few hours researching, configuring, and cursing before asking myself “why am I still bothering with Exim?” I don’t need massive configuration, I just need something to accept mail and toss it straight to /dev/null. So my first thought? “Oh, I’ll just install Sendmail.”

Anyone familiar with Sendmail knows that massive configuration is needed to get it to do anything. I had a base configuration for my servers, but it really wasn’t what I needed. Using the default config I tried the loadtest again, only to have it fail telling me “twilluser12123@localhost does not exist.”  
After about 10 minutes I came to my senses and realized that I really didn’t want to configure sendmail properly because I didn’t know how long it would take and I couldn’t recall how to set up a catchall address at 1am.

When I got up the next morning, I installed postfix (another MTA like sendmail and exim4) that I’d used previously- I remembered it being relatively easy to set up a catchall address. Fortunately it was easy to set up, but I feared having the same connection limitation issues as exim’s default config.

I fired up the jmeter script with 70 threads, 210 second rampup, 100 loops, and an empty database. Each thread loop creates and activates and account, logs in and views it’s edit account page, then creates and views 5 snippets. The test completed **successfully!**

Here are the stats from the test:

8. 7k users were created
9. 35k snippets were created
10. 189k samples were taken
That’s a lot… so what did we find?

## JMeter

- 12 ms Average page return
- 7 ms Median page return
- 490 ms Max page return
- 0 errors in assertions.
- 224.7 hits/second
- 1082.1 KB/second throughput

## Tomcat

15. 239M of Tomcat memory in use (max)
16. 332M of Tomcat memory allocated (max)
17. 30% CPU (max)
18. 90 threads in use (max)
19. 6,477 classes loaded (max)
There were only 2 errors in the tomcat logs:

```
Dec 11, 2010 11:06:34 AM org.apache.catalina.core.StandardWrapperValve invoke
SEVERE: Servlet.service() for servlet dispatcher threw exception
java.lang.IllegalStateException: removeAttribute: Session already invalidated
	at org.apache.catalina.session.StandardSession.removeAttribute(StandardSession.java:1236)
```

```
Dec 11, 2010 11:06:34 AM org.apache.catalina.core.StandardHostValve custom
SEVERE: Exception Processing ErrorPage[exceptionType=java.lang.Exception, location=/WEB-INF/errors/uncaught-error.jsp]
java.lang.IllegalStateException: Cannot reset buffer after response has been committed
	at org.apache.catalina.connector.Response.resetBuffer(Response.java:691)
```

Note that the performance metrics seem better than they should be since I’m testing locally and there’s no network latency. At this point I’m pretty sure the app is performing above my needs, so I have a couple of options:

- Write more functionality
- Improve quality of jmeter testing
- Improve quantity of jmeter testing
- Push existing tests even further to find out where it will break (90 threads? 120 threads?)

I’m not sure what I’m going to do yet.