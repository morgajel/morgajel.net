---
author: Jesse Morgan
categories:
- Uncategorized
date: "2016-04-12T13:11:26Z"
guid: http://morgajel.net/?p=1712
id: 1712
title: Puppet Enterprise + firewall = pain.
url: /2016/04/12/1712
---

I’ve been tasked with setting up puppet enterprise. For numerous reasons it’s shaping up to be the project from hell (some the fault of puppet, but many that aren’t), but I’d like to share this little tidbit for posterity.

The main issue I’ve run into is that our puppet server is in a highly restricted vlan with no internet access. Since puppet pulls its modules from puppetforge, this becomes problematic. The solution we came up with is to explicitly state the git repo to use for each module in the Puppetfile.

# Problem 1: Naming conventions.

I can’t keep 100% fidelity on the projectnames when we migrate them over- for the puppetmodule KyleAnderson/consul, I don’t want to create a KyleAnderson user, so I have to mangle it to merge the user and project name together (since project names alone may not be unique; e.g. if bob/ntp wrote his module for windows and kevin/ntp wrote his module for linux, we can’t just call either puppet/ntp or we’ll get a collision.

We go from this:

```
forge "http://forge.puppetlabs.com"
mod "KyleAnderson/consul", :latest
mod "arioch/redis", :latest
...
```

to

```
forge "http://forge.puppetlabs.com"
mod "KyleAnderson/consul", :latest
  :git => 'https://internalgit/puppet/KyleaAderson-consul'
mod "arioch/redis", :latest
  :git => 'https://internalgit/puppet/arioch-redis'
...
```

In order to do this, we needed to get the git repo for each and mirror it. Well, that was the intent.

# Problem 2: Names don’t match

KyleAnderson/consul does not exist on github. After manually searching [the forge](https://forge.puppet.com/KyleAnderson/consul), I see his URL is actually [solarkennedy/consul](https://github.com/solarkennedy/puppet-consul). So this means we need to get the project URL for each module to be able to clone the git repo. After much experimentation with puppet help module, I realized I can search for the module name, export as yaml and grep out the project name. I end up using the following command to check out the 51 modules I need:

```
for i in `cat .file |sed -e 's/.*"\(.*\)".*/\1/'`; do puppet module search ${i} --render-as=yaml |grep project_url |sed -e 's/.*: //' |xargs git clone ; done;
```

# Problem 3: Inconsistent project URLs

…except that only works for about 80% of the modules- the rest have bad urls. Oh well, 43 is better than nothing.

ok, I have the modules now, time to check them into my git repo…

# Problem 4: can’t check modules into git without the project existing first.

I have to create all 43 projects in the github enterprise web interface; that’s painful. I search and find documentation that eventually leads me to this little nugget:

```
for i in `ls` ; do curl -u "jmorgan3:$token" http://internalgit/api/v3/orgs/puppet/repos -d '{"name": "'${i}'"}' ; done
```

which creates 43 glorious repos. Then, I set the origin URL to my server:

```
for i in `ls` ; do cd $i ; echo $i ; git remote set-url origin git@internalgit:puppet/${i}.git ; cd ~/Projects/puppetmods/ ; done
```

and finally push them up

```
for i in `ls` ; do cd $i ; echo $i ; git remote -v ; cd ~/Projects/puppetmods/ ; done
for i in `ls` ; do cd $i ; echo $i ; git push ; cd ~/Projects/puppetmods/ ; done
```

Now I have all 43 modules checked into my internal git server.

I need to match up repos with modules (since the names may not match).

# Problem 5: Repos were horribly named.

By using the repo names from the project URL, I still ended up with names like realmd, puppet-wordpress, and sssd. Hopefully this won’t bite us later.

I’ve commented out the remaining 7 unmatched projects, committed and pushed my Puppetfile changes, and am now rerunning “r10k deploy environment -pv”

Fingers crossed that this will work.

# Problem 6: Bad syntax, I guess?

There were 100 little syntax issues with the Puppetfile. While I fixed most, this one was not resolvable:

```
# r10k deploy environment -pv
INFO -> Deploying environment /etc/puppetlabs/code/environments/master
INFO -> Environment master is now at 2481f9677469711705bcdb20dd9f0260466b955d
ERROR -> Failed to evaluate /etc/puppetlabs/code/environments/master/Puppetfile
Original exception:
wrong number of arguments (3 for 1..2)
INFO -> Deploying environment /etc/puppetlabs/code/environments/production
INFO -> Environment production is now at a6a7d5eda88334b0293d8534de81191a1375cf06
ERROR -> Failed to evaluate /etc/puppetlabs/code/environments/production/Puppetfile
Original exception:
wrong number of arguments (3 for 1..2)
```

# Problem 7: The control Repo changed!

Between originally checking this out 3 weeks ago and now, they have gutted and rebuilt[ the example](https://github.com/puppetlabs/control-repo) I was using. The rationale makes total sense (it was over-opinionated previously), but now the new version is incomplete, so I’m left twisting in the wind.

I have a call with our puppet reps scheduled shortly and will pick up there.