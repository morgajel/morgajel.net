---
author: Jesse Morgan
categories:
- Development
- Hobbies
- Open Source
date: "2007-08-04T17:18:49Z"
guid: http://morgajel.net/2007/08/04/214/
id: 214
title: Dissappointing state of LDAP support in Ruby
url: /2007/08/04/214
views:
- "82"
---

So I’ve been working on a new project called [Ruma](https://rubyforge.org/projects/ruma/)(more on it later), and I’m beginning to get frustrated with the LDAP support. So far I’ve found  
\* [ruby/ldap, ruby-ldap, ldap-ruby](http://freshmeat.net/projects/ruby-ldap/) by ian macdonald, last release 8/2006  
\* [net-ldap, Net::LDAP](http://rubyforge.org/projects/net-ldap) by Francis Cianfrocca, last release 8/2006  
\* [ruby-activeldap, ActiveLDAP-ruby](http://rubyforge.org/projects/ruby-activeldap) by Will Drewry, last release 5/2007

ldap-ruby appears to be the frontrunner, but hasn’t been updated in a year- the forums appear to think it’s been abandoned. Net::LDAP appears to be a work in progress that isn’t quite complete. ActiveLDAP-ruby appears to be mainly for rails, so I don’t know how useful it is.

When you search for ruby ldap, you can any of those name combinations, and it’s impossible to figure out which is alive, which is stable, and which is useful. At this point I don’t think any of the projects meets all three criteria at the moment.