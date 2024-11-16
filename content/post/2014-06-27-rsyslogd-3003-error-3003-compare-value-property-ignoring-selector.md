---
author: Jesse Morgan
categories:
- Uncategorized
date: "2014-06-27T10:08:13Z"
guid: http://morgajel.net/?p=1541
id: 1541
title: 'rsyslogd-3003: error -3003 compare value property &#8211; ignoring selector'
url: /2014/06/27/1541
---

If you ever come across this message:

```
Jun 27 10:52:52 detc6ut002 rsyslogd: [origin software="rsyslogd" swVersion="5.8.10" x-pid="30899" x-info="http://www.rsyslog.com"] start
Jun 27 10:52:52 detc6ut002 rsyslogd-3003: error -3003 compare value property - ignoring selector [try http://www.rsyslog.com/e/3003 ]
Jun 27 10:52:52 detc6ut002 rsyslogd: the last error occured in /etc/rsyslog.conf, line 65:":programname, regex, 'ASA-[65432]-' ~"
Jun 27 10:52:52 detc6ut002 rsyslogd: warning: selector line without actions will be discarded
Jun 27 10:52:52 detc6ut002 rsyslogd-2124: CONFIG ERROR: could not interpret master config file '/etc/rsyslog.conf'. [try http://www.rsyslog.com/e/2124 ]
```

Make sure you’re using [double quotes](http://www.gossamer-threads.com/lists/rsyslog/users/3518) on your regex string.

```
:programname, regex, "ASA-[65432]-" ~
```

This took me far longer than it should have to figure out. Let me know if you find this helpful.