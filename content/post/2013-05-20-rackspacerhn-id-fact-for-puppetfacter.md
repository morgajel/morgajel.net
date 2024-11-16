---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-05-20T14:17:00Z"
guid: http://morgajel.net/?p=1421
id: 1421
title: Rackspace/RHN ID Fact for Puppet/Facter
url: /2013/05/20/1421
---

Having your RHN/Rackspace ID available as a fact is very useful. Here’s an “easy” way to get it (just don’t ask me how long it took me to wrestle it out of REXML).

```
# rackspaceid.rb

Facter.add("rackspaceid") do
  setcode do

    require "rexml/document"
    xml = File.read('/etc/sysconfig/rhn/systemid')
    root= REXML::Document.new(xml)
    rsid=1
    root.elements['params/param/value/struct'].each  do  |member|
        if member != "\n" and member.elements['name'].text == 'profile_name'
            rsid = member.elements['value/string'].text
        end
    end
    rsid
    # puts rsid

  end
end
```