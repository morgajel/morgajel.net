---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Rant
- Work
date: "2006-02-02T06:38:24Z"
guid: http://morgajel.net/?p=45
id: 45
title: bit-slapped by karma
url: /2006/02/02/45
views:
- "38"
---

So i’m getting slapped around a bit by karma right now.

I recently moved our DNS services to two new servers. everything appeared to be fine… I got cocky.

Now it turns out they’re not behaving like proper slaves for certain domains. fore xample, we run foo.com, bar.com and bar.net as the primary DNS. We ALSO act as slaves for baz.org… except we’re not anymore. For some reason, we’re not getting updates from the primary DNS for baz.org. We’re not sure why.  
———————-  
Next up, We have another DNS server for a remote office- it’s behind a nasty firewall that lets us get it, but nothing from it can get out- including the package manager. This means the machine cannot get security updates. Obviously this is not an ideal solution, so I went on a 3 day wild goose chase first trying to get internet access from it (not allowed), finding out that we \*could\* use a proxy, finding out that no one knows who owns the proxy, tracking down the people responsible and finding out we have to go in front of a comittee (no kidding) to get permission to use this proxy. So what do I do?

I decide to get clever.

Since I can ssh into the machine, it stands to reason I can scp the files to it- but if I can scp files to it, that’s almost like using my machine as a proxy… but this doesn’t help if up2date (redhat package management tool) needs network access to register itself… but since I’m already acting as a proxy…

So here’s what I did- I turned my laptop into a REAL proxy. I created an SSH tunnel that listened to port 2221 on the remote server and had it dump it to port 2222 on my laptop. I installed Squid proxy on my workstation listening to port 2222, then told up2date use use localhost:2221 as a proxy server.

And the amazing part is it worked!

I was able to register up2date and patch the machine. I was happy… I then got cocky.

 I was comparing the package list of this DNS server to the other two I had set up, looking for inconsistentcies. Three packages would not update- openssh, openssh-client and openssh-server. I looked closer and noticed they had an unusual version number. I asked the previous DNS admin what he knew about them, and he responds that a while back there was a nasty hole in ssh that needed to be patched, and RH didn’t have patches available, so he used some non-standard RPMs. Now up2date was seeing them and freaking out, refusing to touch them. I couldn’t even get up2date to download the correct packages. So I came up with a plan.

I fired up screen and created a script that would do the following: remove the troublesome packages with rpm, then run up2date and grab the new packages. I did it in a screen shell just in case it killed my ssh connection. Seemed like a reasonable plan, right?

Had I thought this through a little better, I would have realized how stupid I am.

My plan would have worked great on any other machine or any other package. the thing I didn’t take into account was I was using the ssh tunnel to run up2date. As soon as it removed ssh it killed the tunnel, preventing up2date from downloading the new ssh packages to install.

And so it sits, in the remote office, with no way to update it. So now I get to take a field trip with one of my coworkers out to the remote facility, about 2 hours away.

I am a tool.