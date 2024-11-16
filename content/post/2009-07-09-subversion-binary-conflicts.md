---
author: Jesse Morgan
categories:
- Uncategorized
date: "2009-07-09T09:23:39Z"
guid: http://morgajel.net/?p=536
id: 536
title: Subversion Binary Conflicts
url: /2009/07/09/536
views:
- "35"
---

I caught some interesting behavior on one of our servers today- I’m using subversion to manage a jboss instance (there’ll be a writeup on this later, I’m sure) and caught the following behavior. Here’s the simplified setup.

Two servers, X and Y. both of which are checked out of the subversion repository as a whole and fired up. They are clustered with each other. Each server is hosting a ROOT.war file (binary) that contains the site’s code base. This file is stored in the farm/ directory, and under normal circumstances, when a file is deployed on one to the farm directory, it’s ‘farmed’ over to the other one.

Eventually I plan on disabling the deployment scanner and farming (probably), but it revealed some interesting behavior in the meantime.

A new version of the code base was given for us to deploy. Since we’re half way though implementing these changes, I deployed per our current convention.

- shut down Y
- deploy to X
- restart X
- start Y

For reasons beyond the scope of this discussion, that’s how we do it to get it to farm cleanly. So now I have a \[M\]odified ROOT.war file on both servers. I commit the file on server X to the repository as revision 537.

At this point, the file on X, Y and in the repository are all the same- same contents, same md5sum, even the same date on the file between X and Y.

The only problem left is Y now has an out of date revision and a modified warfile- It still thinks it’s on rev. 536.

Had this file been a simple text file, running svn up would have noticed the local modifications were the same as the repo changes, and would have mer\[G\]ed the changes. Unfortunately svn doesn’t know what to do with binary files and treats them like they’re \[C\]onflicting, resulting in the following three files (md5sums included for illustration):  
`<br></br>8414d634a7afabade693303ad3e2b2c5  ROOT.war<br></br>5faa76d385cac6b2297069fbfbf49bd5  ROOT.war.r536<br></br>8414d634a7afabade693303ad3e2b2c5  ROOT.war.r537<br></br>`

ROOT.war is the current version that’s there- the one that was farmed over from X. .r536 is the previously mentioned version of the file, and r537 is the latest version from the repository.

Now, under normal conditions, this isn’t a problem… But remember, this is happening in the farm/ directory, so these two new versions may be passed out to the cluster (X). This isn’t optimal.

How could it be more cleanly resolved (no svn pun intended)? subversion could do an md5sum on binary files to see if they’re “the same”, and if so, merge it rather than mark it as \[C\]onflicted.

I’m pretty sure I’m running an older version of subversion, so this functionality might already be there, but if it isn’t, it’d be a great feature to add.