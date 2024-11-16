---
author: Jesse Morgan
categories:
- IRC
- Linux
- Open Source
- Work
date: "2006-07-13T12:24:50Z"
guid: http://morgajel.net/2006/07/13/144/
id: 144
title: More IE Fun
url: /2006/07/13/144
views:
- "58"
---

ran into a problem with mod\_rewrite and IE- a real one this time. IE was choking on files that were being downloaded via mod\_rewrite. same script worked without rewrite, died with it.  
Did some research and with a lot of help from noodl of #apache figured out it was because mod\_rewrite adds a “Vary: Host” line to the header, which apparently IE chokes on. loaded the header module in apache and added “Header unset Vary” after my rewrite and all is good- cvs, pdf, zip all download properly. Hurray.