---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-10-30T10:18:35Z"
guid: http://morgajel.net/?p=1451
id: 1451
title: Find out the DN for a user in Active Directory
url: /2013/10/30/1451
---

It is usually a pain in the butt to figure out how to find the DN of a user and I can never remember how I did it; Fortunately I stumbled across [this link](http://wiki.zimbra.com/wiki/LDAP_Active_Directory) which gives the following advice:

> You may be asked to define a DN so that a service can bind to it to authenticate a query. Each user in Active Directory has a distinguished name. However, you cannot find it through the ADUC tool.
> 
> From a command prompt on your domain controller type: **ldifde -f c:\\export.txt**
> 
> View the export.txt file in Notepad and do a find on the username. For example, you do a find on username zimbrauser. You will see something like this:  
> CN=zimbrauser,OU=External,DC=exonline,DC=intranet
> 
> This means that zimbrauser is in the OU called External in your AD forest exonline.intranet.

This worked beautifully for me. I’ll admit that it is a bit of a kludge, but damn… it’s so simple.

As usual, if this was helpful, leave a comment telling me so.