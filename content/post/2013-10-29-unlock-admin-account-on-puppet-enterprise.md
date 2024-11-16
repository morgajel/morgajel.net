---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-10-29T15:44:36Z"
guid: http://morgajel.net/?p=1444
id: 1444
title: Unlock Admin account on Puppet Enterprise Via the Command Line
url: /2013/10/29/1444
---

## Long Version

Backstory time! I’m currently in the process of implementing a new Configuration Management system. Since this will only cover a small subsection of servers, I opted to go with Puppet Enterprise (PE). As part of the installation, it asks you for your email address to use as a username- I used my work address. After getting it installed and configured, I thought it would be a good idea to auth off of Active Directory; I forgot to disable the internal account when doing this, meaning there were two jmorgan@foo.com that were possible; by the time I had figured this out, puppet had “locked” my account; I disabled the AD auth to confirm that yes, jmorgan@foo.com was indeed locked.

So, how do you unlock the admin account? After spending a couple of hours googling and asking on freenode’s #puppet channel, I decided to brute force it. By default, PE uses its own postgres database, so I figured out how to crack that open, then dug through the tables and fields, then through the PE sourcecode until I found the secret…

## Short Version<span style="font-size: 0.8em;"> </span>

1. Find the password for console\_auth here in **/etc/puppetlabs/installer/database\_info.install**.
2. Connect to the internal postgres DB (presuming you used it): **psql -h localhost -p 5432 -U console\_auth console\_auth** using the password found in the file above.
3. Run the following SQL command: **update authorized\_users set status=’enabled’, login\_failure\_count=0 where username=’jmorgan@foo.com’;**

And that’s it- all that gnashing of teeth for three little steps. Now I can change the admin account to something less conflicty, and retry the AD authentication.

If this saves your butt, please let me know- I couldn’t find this documented anywhere.