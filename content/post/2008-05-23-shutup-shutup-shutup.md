---
author: Jesse Morgan
categories:
- Linux
- Rant
date: "2008-05-23T12:34:25Z"
guid: http://morgajel.net/2008/05/23/247/
id: 247
title: Shutup Shutup Shutup!
url: /2008/05/23/247
views:
- "55"
---

die you wretched pc speaker beep that is so loud when I use the find bar in firefox or tab complete in the cli and nothing is found!

```

  rmmod pcspkr
  echo blacklist pcspkr >> /etc/modprobe.d/blacklist
```

aaah, glorious silence.