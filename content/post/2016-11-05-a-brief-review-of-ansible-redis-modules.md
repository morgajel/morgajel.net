---
author: Jesse Morgan
categories:
- Uncategorized
date: "2016-11-05T23:29:22Z"
guid: http://morgajel.net/?p=1720
id: 1720
title: A Brief Review of Ansible Redis Modules
url: /2016/11/05/1720
---

I’m currently investigating the best ansible module to manage redis for my server. The good news is that ansible galaxy has plenty of options; the bad news is that most of them are terrible. This is my first attempt to find the best of the bunch.

For the sake of simplicity, I’m limiting my search to roles that support Enterprise\_Linux (e.g. Redhat, Centos, etc). In addition, I’m going to be examining the github repos rather than the galaxy entries.

It’s important to note that I’m not judging the authors, only their usefulness to me.

# https://github.com/hostclick/redis

Last Commit: Sept 15th, 2015

Commits: 2 Contributors: 1

Branches: 1 Releases: 0

Pros:

- Default values used
- Remi repo used
- config templatized
- vars used

Cons

- Installs its own Remi repo config
- docker stuff included
- extensive template hardcodeds content
- README example is limited.

# https://github.com/jtyr/ansible-redis

Last Commit: May 25th, 2016

Commits: 15 Contributors: 3

Branches: 1 Releases: 0

Redis versions supported explicity: 2.4, 2.6, 2.8

Pros:

- Extensive defaults
- simple tasks and template
- Estensive README

Cons:

- overly simplistic module, complex variables
- uses default redis package

# https://github.com/officel/ansible-role-redis

Last Commit: September 8th, 2016

Commits: 5 Contributors:1

Branches: 1 Releases: 0

Pros:

- includes spec file
- enables remi and epel repos

Cons:

- includes docker for tests
- doesn’t include repos as requirements

# https://github.com/sbaerlocher/ansible.redis

Last Commit: September 27th, 2016

Commits: 7 Contributors: 1

Branches: 1 Releases: 3

Pros:

- Good Defaults
- Excellent README
- multilayer vars configuration
- includes test playbook and inventory
- Supports multiple distributions

Cons:

- complex vars configuration
- default packages only, no repo support

# https://github.com/AerisCloud/ansible-redis

Last Commit: June 20th, 2016

Commits: 5 Contributors: 3

Branches: 1 Releases: 3

Pros:

- includes good repo dependencies

Cons:

- Poor defaults
- Bad formatting with redirects
- Bad README

# https://github.com/mrlesmithjr/ansible-redis

Last Commit: June 7th, 2016

Commits: 18 Contributors: 1

Branches: 1 Releases: 0

Pros:

- includes performance tweaks

Cons:

- includes docker file
- bad defaults
- mentions epel, no include or dependencies
- no repo dependencies
- Poor vars

# https://github.com/dgnest/ansible-role-redis

Last Commit: March 10th, 2016

Commits: 36 Contributors: 1

Branches: 2 Releases: 6

Pros:

- includes build status

Cons:

- No repo dependencies
- Weird tasks layout
- Configuration not really EL specific (more debian than Redhat)

# Results

Wow…. that was, uh, painful. The good news is a lot of them are still active, though the number of commits is relatively low. across the board. The low commit numbers could mean one of two things:

1. Ansible roles are easy to get right the first time, or
2. they’re slapped together and not really polished.

There’s a few we can rule out straight away: mrlesmithjr, dgnest, AerisCloud- there just wasn’t a lot of useful content.

That leaves hostclick, jtyr, officel, and sbaerlocher with useful content. I think the right answer will be to roll my own taking parts from each. I’ll give it a closer look tomorrow.

Update: AAAND I feel dumb. I didn’t notice during my first search that those were the first 10 results- 3 rows of 3 and one row of 1 made it look like that was the end of the list.

I’ll have to re-evaluate, probably based on “most downloaded.”