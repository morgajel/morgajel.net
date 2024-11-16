---
author: Jesse Morgan
categories:
- Uncategorized
date: "2012-03-06T10:48:52Z"
guid: http://morgajel.net/?p=1194
id: 1194
title: NTP Querying
url: /2012/03/06/1194
---

Setting up a new NTP client? Can’t tell if it’s syncing properly? Use

```
ntpq -c lpeers
```

to figure out if things are syncing properly.