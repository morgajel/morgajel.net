---
author: Jesse Morgan
categories:
- Uncategorized
date: "2012-08-16T08:37:24Z"
guid: http://morgajel.net/?p=1218
id: 1218
tags:
- jenkins
title: Mark a job unstable in Jenkins from a shell script
url: /2012/08/16/1218
---

Here’s the workaround I cam up with

Configure JUnit reports to read status.xml and output something like this:

```
echo '<testsuite><testcase classname="yourJob" name="yourError"><failure>
a description of the error
</failure></testcase></testsuite>'>status.xml
```

Simple as that!