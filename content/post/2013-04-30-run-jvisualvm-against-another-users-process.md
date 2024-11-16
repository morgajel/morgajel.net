---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-04-30T15:22:57Z"
guid: http://morgajel.net/?p=1396
id: 1396
title: Run JVisualVM against another user&#8217;s process
url: /2013/04/30/1396
---

Suppose you’re a good admin and you run JBoss as it’s own user rather than as root or yourself, but your devs need to use VisualVM- How do you give them access without providing ssh access to jboss or setting up jstat? Today I figured out I could do this:

First, install the required packages to forward X11 traffic:

```

  yum install xorg-x11-xauth libXtst
```

Once that’s in place, make sure that you have the following in your server’s sshd\_config:

```

  X11Forwarding yes
  AllowTcpForwarding yes
```

And in sudoers, make sure you have:

```

Defaults env_keep += "DISPLAY XAUTHORITY XAUTHORIZATION"
```

The final step is to use some Xauth voodoo. I wrote this handy wrapper script to bottle up the changes:

```

#!/bin/bash
MYID=`/usr/bin/xauth list|head -n 1|awk '{print $1}'`
/usr/bin/xauth extract - ${MYID} |/usr/bin/sudo -u jboss -H /usr/bin/xauth merge -
cd /tmp/
/usr/bin/sudo -u jboss -H -E /usr/java/latest/bin/jvisualvm
```

And as long as your users have the proper sudo access to the jboss user, you should be all set.

Let me know if this was useful.