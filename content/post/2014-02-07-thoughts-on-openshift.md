---
author: Jesse Morgan
categories:
- Uncategorized
date: "2014-02-07T12:45:57Z"
guid: http://morgajel.net/?p=1474
id: 1474
title: Thoughts on Openshift
url: /2014/02/07/1474
---

When I began work on Megacosm Generator, I decided to look into Application platforms for hosting. My requirements were relatively low:

- Free
- Runs Python

While I had started with Google App Engine, I found that it could not use non-python python modules (i.e. python modules written in C++), such as the Noise module that I was using to define landmasses. After examining several options, I decided to give Openshift a try.

While Openshift did in fact work, it was riddled with problems, most of which were simply bad design issues.

## Git Grief

When creating a project, you have the chance to designate a github repository to pull from. This however will fail if you use ssh; This would be nice to know before waiting a minute for it to try to create the project, only to have it fail with that notification. Throw a javascript warning saying “this url should start with https://” and make the user experience better.

Despite setting up the github url, it only clones the repo rather than set up any type of scheduled pull, which would have been nice. This means that now rather than managing one remote repository, you’ll need to manage two, but they’re all the same so it’s not a big deal, right?

Well, unless you use git flow and work out of a develop branch. As far as I’ve found, there is no way to switch branches in openshift- which means if you want to see your local develop branch on openshift, you have to merge it into openshift’s master branch. This workflow just feels… wrong.

To at least get a proof of concept working, I ignored the git repo setup and let it generate it’s own, the copied my files from the github checkout to the other- it was amateur, but it worked. After much wrestling, tweaking, cajoling and swearing, I was finally able to get it working.

## It’s SLOOOOW

When I finally get get all the kinks worked out and got everything functional, I was disappointed by how ungodly slow it was to render my maps on the planet. On my local machine, it took 5.5 seconds to render the entire planet- most of that was due to waiting for three separate perlin noise maps on the backend.

In comparison, Openshift took over 30 seconds. It was so slow at first that I thought the app still wasn’t working. Even 30 seconds is a guess; that’s how long I waited before going back debugging, reuploading, and then checking back to see that it was in fact finally rendered. To be honest, it’s not really worth the effort to replicate because…

## It’s Unreliable

After uploading for the billionth time, it still wasn’t working, and I couldn’t figure out why. There was little feedback in the web interface to indicate there was a problem. After checking the HA-proxy interface, I saw the node was down for maintenance. “No problem,” I thought, “This is why it has HA Proxy- I can spin up multiple instances on the backend” I spun up another two “gears” and waited… and waited. I stopped the gears. I started the gears. I restarted the system… nothing. Everything was still down. It had been an hour since it had originally went down and I was growing frustrated. I kept trying over the course of the night, eventually deleting the entire thing and trying again. No dice. What’s the point of a scalable system like this if it’s this fragile?

## In Conclusion…

I’ve decided that at this point, openshift isn’t worth the effort. Part of the problem is admittedly my ignorance of the platform and ignorance of Python in general. However, Openshift’s poor interface feedback coupled with questionable stability and horrible performance is too much for me to take at this time. Once I un-kludge my configuration, I’ll look into other options. If Openshift goes through any major changes, I’d be willing to give it another shot.