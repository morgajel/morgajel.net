---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-06-10T09:18:52Z"
guid: http://morgajel.net/?p=1429
id: 1429
title: 'Resolved: JBoss EAP 6 threads leaking with Mod_Cluster?'
url: /2013/06/10/1429
---

Ever notice that your new EAP implementation appears to be leaking threads? Thread dump pointing to AJP? Does your thread usage resemble the graphs on the top, continuously climbing?

[![threads](http://morgajel.net/wp-content/uploads/2013/06/threads1.png)](http://morgajel.net/wp-content/uploads/2013/06/threads1.png)

Fortunately, the solution is simple- JBoss and mod\_cluster are too brain damaged to account for Apache HTTPD timing out threads and closing them down, so it leaves them having open forever rather than have a sane (or even insane) default timeout. To work around this, add the following to your system-properties in your host.xml, standalone.xml or domain.xml:

```
<system-properties>
 ...
<strong> <property name="org.apache.coyote.ajp.DEFAULT_CONNECTION_TIMEOUT" value="600000"/></strong>
 ...
 <system-properties>
```

Poof, after 10 minutes of idle time, JBoss kills the thread, resulting in the graphs on the bottom after a restart. Leave a comment if this was helpful.