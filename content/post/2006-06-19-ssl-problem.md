---
author: Jesse Morgan
categories:
- Uncategorized
date: "2006-06-19T09:03:13Z"
guid: http://morgajel.net/2006/06/19/132/
id: 132
title: SSL problem
url: /2006/06/19/132
views:
- "51"
---

So here’s the problem: We’re setting up a new mailserver for our customers at work and I’d like for them to use SSL on their imap connections- the problem is we don’t want to get an SSL cert for each of the domains (there are around 30 and it’s constantly changing). After talking to the ssl people, they said that getting a cert for the ip would be the best solution.

so our mailserver (mail1) has an SSL cert for 123.123.123.123. Everything seems to work in outlook express, which is what \*most\* of our customers use…

BUT… kmail on linux and mail on OSX seem to alert that “The IP address of the host mail1.ourdomain.com does not match the one the certificate was issued to.”(kmail message). when I click on details, there appears to be 3 levels to the “chain” of ssl certification  
\* Site Certificate  
\* 123.123.123.123  
\* UTN-USERFirst-Hardware

From what I can see, Site Certificate is identical to 123.123.123.123, except “The Certificate has not been issued for this host.”

Short of getting an SSL cert for each one of these domains, and adding a new one each time we add a new domain, can we make this error go away? I’m looking for a universal solution for all clients if possible