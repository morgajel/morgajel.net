---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-04-12T08:04:48Z"
guid: http://morgajel.net/?p=1393
id: 1393
title: Puppet Training Notes from Day 3.
url: /2013/04/12/1393
---

Final notes from day 3:

1. Using an External Node Classifier(ENC) is considered best practice over site.pp
2. There is a “puppet visual index” that has useful examples
3. You can use ~&gt; and -&gt; to chain dependencies to help make things more readable.
4. If I can figure out how to integrate it, I could use LDAP inventory tags to indicate puppet classes
5. Hiera can be used to extract out data from configs
6. Once data is abstracted out, I can put them on github and puppetforge
7. the config\_version parameter can be used to control the format of the puppet version (using date command, for example)
8. There is a style guide available for puppet- it’s what puppet-lint uses.
9. there is a vim-puppet plugin on github, however it’s currently broken in apt on ubuntu
10. syntastic may be worth checking into to make vim a better IDE
11. I really need to reconfigure ctags for perl.
12. I should use case default: { fail(“this ${module\_name} isn’t supported here”)} for better errors on case statements
13. ask.puppetlabs.comn is a great resource when IRC fails me.
14. I can use rspec with puppet to do TDD. This sounds very promising.
15. Certification is $200, however I get get a 25% discount. I should be able to pass this.
16. If puppet dashboard is slow, clean out old reports.
17. Consider signing up for the test pilot program.