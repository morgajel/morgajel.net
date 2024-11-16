---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Rant
- Work
date: "2006-07-10T15:30:36Z"
guid: http://morgajel.net/2006/07/10/142/
id: 142
title: Rage against the IE.
url: /2006/07/10/142
views:
- "71"
---

ok, here’s my latest bout of IE stupidity. The new system I’m setting up will have several developers working on several projects. We’d like to be able to use subversion to manage the projects without dealing with pathname stupidity, hence all new projects should have / as their base since they will later become full fledge sites. So, how do we do that? I had the simple idea of mapping http://foo.jmorgan.devel.oursite.com to http://devel.oursite.com/~jmorgan/foo/ with a mod\_rewrite:  
**Note:** I’ve since removed virtualhost stuff.

```


    RewriteEngine on
    RewriteLogLevel 9
    RewriteLog "/var/log/rewrite.log"
    RewriteCond %{HTTP_HOST} ^(.+)\.(.+)\.devel.oursite.com$  [NC]
    RewriteRule ^(.*)$  /~%2/public_html/%1/$1 [L]
    ...
```

This works awesome…except for IE. You see, we need to set a cookie, and IE chokes on this. Why? I’m not sure. We’ve tried setting the domain of the cookie to every possible combination we could think of; modifying the heck out of the rewrite did no good either.  
This works in Firefox, Opera, Konqueror, etc, but not IE… We think this is due to the redirect domain being different that the issuing domain, but since IE never sets the cookie, we can’t tell. To add to the complexity, I’m using Terminal server to test in IE since I’m running Linux and that limits my flexibility.

So, anyone know why this stupid thing is so stupid?