---
author: Jesse Morgan
categories:
- Uncategorized
date: "2006-05-30T13:30:32Z"
guid: http://morgajel.net/2006/05/30/128/
id: 128
title: a mail dilemma
url: /2006/05/30/128
views:
- "49"
---

So here’s the situation- We’re trying to consolidate our 5 mailservers into 1 master server with as much transparency to the users as possible. We have around 130 domains with a total of about 640 users. Some domains have one user, some have 50.

The first step of the migration was to determine the new setup. Since all of the servers have completely different setups, we can be flexible.  
We settled on Courier-imap, postfix, amavid, clamav, spamassassin, squirrelmail and maildir. Now we just have to move everything onto it.

The second step of the migration was to move accounts on our biggest mailserver(mailfilter) over to the new server(newmail). We plan on doing this one domain at a time, starting with the smallest number of users and working up to the more populated domains.  
Mailfilter used the following: uw-imap, postfix, mailscanner, clamav, spamassassin, openwebmail and mbox.

Now, I’ve managed to get everything configured properly, and was even able to migrate two 1 person domains (my coworkers) to the new server. I wrote a script to migrate mail from mbox to maildir format and even have sane mail creation dates. Now all I do is simply point their mx record to the new server and poof, everything works….on the server side.

The problem is this: (I don’t know much about imap, so most of this is theory…)  
Suppose user Dan is running Outlook or Outlook Express. We change over his DNS, and he checks for new mail. Suddenly he’s getting all sorts of new errors. My theory is that imap is caching the headers of all the emails, and when it switches to the new server, the headers aren’t exact matches, and hence has 2 entries for each email- 1 defunct entry from the old server that errors when you click on it, and 1 working entry. Now, if the user removes the account from Outlook/OE, then re-creates it, everything works fine- the problem is we want to make this transparent to the users.

So how do we force the imap cache to clear from the server?