---
author: Jesse Morgan
categories:
- Uncategorized
date: "2014-07-02T12:31:53Z"
guid: http://morgajel.net/?p=1544
id: 1544
title: The Case of the Truncated Syslog Program Field
url: /2014/07/02/1544
---

# The Problem

Data was going in to my logging server and getting mangled somewhere along the line. To complicate matters, only the windows hosts were affected, and even then it was sporadic. The truncated data was in the middle of the string, which left me to believe logstash was trying (and failing) to parse it.

To fully grasp my setup, perhaps a diagram is in order:

<div class="wp-caption alignnone" id="attachment_1545" style="width: 286px">[![IMG_20140627_094205](http://morgajel.net/wp-content/uploads/2014/07/IMG_20140627_094205-e1404320597698-276x300.jpg)](http://morgajel.net/wp-content/uploads/2014/07/IMG_20140627_094205.jpg)My wonderful pocket logserver

</div>The flow went something like this:

```
NXLog -> Rsyslog -> Logstash -> Redis -> Logstash -> Elasticsearch -> Kibana
```

When manifested, this was spit out of rsyslog into a temp logfile:

```
Jul 2 10:40:25 dep1w1us001.derp.com Microsoft-Windows-TerminalServic<strong>es-RemoteConnectionManager[2272]:</strong> Remote Desktop Services: User authentication succeeded: User: jesse.morgan Domain: derp Source Network Address: 192.168.6.38
```

And this is what ended up in rediss (note the missing bold):

```
Jul 2 10:40:25 dep1w1us001.derp.com Microsoft-Windows-TerminalServic Remote Desktop Services: User authentication succeeded: User: jesse.morgan Domain: derp Source Network Address: 192.168.6.38
```

For some reason, the middle of the line was missing!

# The Culprit

With the help up my trusty sounding board Will, “Logstash guru” thegreenrobot, and nxlog dev b0ti, I was able to figure out the root cause- the problem wasn’t with Logstash at all, it was broken when rsyslog handed it to logstash. Complicating matters was **when** rsyslog was munging it- when it wrote the entries to a temp file, they were fine, but when it passed them to logstash, it truncated them on the way out the door.

Why? Because they’re invalid in the first place!

NXLog (the logging forwarder on windows) was collecting eventlog data and shipping it out with the full program names despite it being [too long](http://www.gossamer-threads.com/lists/rsyslog/users/8779). That’s right- [Section 4.1.3 ](http://tools.ietf.org/html/rfc3164#section-4.1.3)of RTF3164 states:

```
<pre class="newpage" style="color: #000000;">This has traditionally been a freeform message that
gives some detailed information of the event.  The TAG is a string of
ABNF alphanumeric characters that MUST NOT exceed 32 characters.
```

While you could (and I still do) consider this a bug in nxlog, b0ti pointed out that fixing this bug could potentially break many stable implementations to fix an edge case. While I don’t fault B0ti for not fixing it, I still needed to work around it.

# The Fix

We need to shrink down that program name to 32 characters or less before it leaves nxlog… but there’s a catch.

It’s not \*just\* the program name, but the PID as well that gets truncated. So we have to adjust accordingly. The [msdn](http://msdn.microsoft.com/en-us/library/ms683180(VS.85).aspx) documentation says the PID is a DWORD, which [turns out](http://msdn.microsoft.com/en-us/library/windows/desktop/aa383751(v=vs.85).aspx#DWORD) to be an unsigned 32-bit integer, which can potentially be 10 characters. Add on another two for brackets and one for a colon and you’re already losing potentially 13 characters of the 32 characters allowed.

To help stretch our remaining 19 characters, lets see what we can axe. Most of the Microsoft processes either begin with “Microsoft\_” or “Microsoft-Windows-“, which we honestly don’t need on the program name. By removing those, we get down to the nitty gritty of the actual program name.

Since it may STILL be too long (I’m looking at you **Microsoft-Windows-TerminalServices-RemoteConnectionManager\[2272\]:**), we need to strip those off the front, THEN take the 19 character substring of what remains. The result is an eventlog input in nxlog that looks something like this:

```
<Input eventlog>
 Module im_msvistalog
 Exec if ($Channel == 'Security') drop(); \
 if ($SourceName =~ /^Microsoft[-_]/) $SourceName = substr($SourceName, 10);\
 if ($SourceName =~ /^Windows[-_]/) $SourceName = substr($SourceName, 8);\
 $SourceName = substr($SourceName,0, 19);
</Input>
```

And with that, rsyslog no longer truncates, logstash can parse the string, and all is right with the world (as far as I can tell.) A big thanks to everyone who’s helped me troubleshoot this issue.

If you found this useful, please let me know in the comments.