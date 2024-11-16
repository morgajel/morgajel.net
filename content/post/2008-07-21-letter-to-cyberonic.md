---
author: Jesse Morgan
categories:
- Uncategorized
date: "2008-07-21T11:06:53Z"
guid: http://morgajel.net/?p=264
id: 264
title: Letter to Cyberonic
url: /2008/07/21/264
views:
- "56"
---

sent at 12:01pm:

> Hi guys, I’ll try to sum this up real quick to get you up to speed:
> 
> Name on Account: Jesse Morgan  
> Issue: Damanged Line/ Connection problems  
> land line: \*\*\*-\*\*\*-\*\*\*\*  
> Contact Number: \*\*\*-\*\*\*-\*\*\*\*  
> Address: \*\*\*\*\*\*\*\*\*\*\*\*
> 
> I’m having line issues- I’ve called both cyberonic and AT&amp;T (before I dropped their service for yours) several times over the last few weeks trying to piece together what is going on. For the record I’ve tried several different DSL modems and routers and have successfully removed them from the equation. Last time I spoke with your team, they confirmed that the link loop (loop link?) was damaged, and it was an issue for the telcos and probably not fixable.
> 
> I’ve talked to my father-in-law who works for a large, nameless telco and he offered the following advice:
> 
> – AT&amp;T (owners of the line to my house) have an obligation to provide covad (the line you are reselling) with an undamaged line.
> 
> – if you contact covad and let them know about the line, they will contact AT&amp;T and demand the line be fixed post haste. While they have duty to do this on your behalf, I suspect they’ll be happy to just to give AT&amp;T a hard time.
> 
> As it stands now, the line is unreliable and unacceptable. I am supposed to work from home on Wednesdays and be on-call 2 weeks a month, and the current connection is too unstable for me to risk it.
> 
> I understand there’s only so much you guys can do, but the connection dropped today around 7:40am when I was supposed to be doing a code deployment to my employer’s main site. Needless to say the people depending on me were not amused.
> 
> I really liked your service when I had it in 2005 in the DC area, however if this isn’t resolved, I may have to switch to cable since you cannot provide “Robust and reliable networking.” I \*want\* to give you my money, I really do- but I need stability.
> 
> Could you please place a ticket with Covad on the line quality? perhaps hint that they need to contact AT&amp;T. I would provide you with an rrd graph showing packet loss and such, but the server hosting said graph is currently on the bad end of the line 🙂
> 
> Thank you for addressing this as speedily as possible. I’m a sysadmin who’s job requires 24×7 access, and I really need to get this up as soon as possible. I’ve been without a stable connection for almost a month now and my employers are getting antsy.
> 
> Sincerely,  
> Jesse Morgan
> 
> P.S. As I wrote this email, I decided to do a quick ping check from work to illustrate the behavior I’m seeing. The first IP is the gateway my router connects to, the second is the ip address of my router:
> 
> jmorgan@zeeble ~ $ ping 66.134.112.129 -q -c 20  
> PING 66.134.112.129 (66.134.112.129) 56(84) bytes of data.
> 
> — 66.134.112.129 ping statistics —  
> 20 packets transmitted, 20 received, 0% packet loss, time 19006ms  
> rtt min/avg/max/mdev = 13.737/15.721/26.443/2.803 ms
> 
> jmorgan@zeeble ~ $ ping -q morgajel.com  
> PING morgajel.com (66.134.112.212) 56(84) bytes of data.
> 
> — morgajel.com ping statistics —  
> 447 packets transmitted, 0 received, 100% packet loss, time 446118ms
> 
> jmorgan@zeeble ~ $

**Update- 12:42pm**

> we have recieved your email and update Ticket ID: \*\*\*\*\*\*. We are esclating  
>  your reported issue and will contact you soon  
>  Thank you.
> 
>  Sincerely,  
>  Dave

**Update – 2:28pm**  
Just got off the phone with Jim- he said he’d checked the line and saw it was down, and they were willing to send someone out to check the line- if it’s an interior problem (unlikely but possible), I’d have to pay for it, otherwise they’d cover it. I offered to have jackie reset the modem, and if that resolves it, he said we could work out something to replace the netopia dsl modem/router with a straight modem, which would rock.

The line has been down since 9am or thereabouts, so hopefully it’ll still be having issues when the technician gets there in the next day or two (they said they’d send the first available technician). Note- Even after jackie restarted the modem, the connection never came back. Hopefully the misbehavior will continue long enough for them to quickly diagnose the issue.

**Update**  
last night called them back, reset the router, went through reconnection process and confirmed there was no signal. tried again this morning before work and no luck. Around 10am it started working again.

Call again this morning, talked to Jim again, had Jackie enable remote management, and he’s gonna watch the error stats over the next few hours or so. Hopefully the connection will drop again while he’s looking.