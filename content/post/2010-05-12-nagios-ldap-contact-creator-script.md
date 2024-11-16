---
author: Jesse Morgan
categories:
- Uncategorized
date: "2010-05-12T15:04:40Z"
guid: http://morgajel.net/?p=701
id: 701
title: Nagios LDAP Contact Creator Script
url: /2010/05/12/701
views:
- "140"
wp-syntax-cache-content:
- "a:1:{i:1;s:19834:\"\n<div class=\"wp_syntax\" style=\"position:relative;\"><table><tr><td
  class=\"code\"><pre class=\"perl\" style=\"font-family:monospace;\">&nbsp;\n<span
  style=\"color: #666666; font-style: italic;\">#!/usr/bin/perl</span>\n<span style=\"color:
  #666666; font-style: italic;\"># read your ldap repo and generation nagios config
  files for contacts</span>\n<span style=\"color: #666666; font-style: italic;\">#
  and contact_groups</span>\n<span style=\"color: #000000; font-weight: bold;\">use</span>
  strict<span style=\"color: #339933;\">;</span>\n<span style=\"color: #000000; font-weight:
  bold;\">use</span> Net<span style=\"color: #339933;\">::</span><span style=\"color:
  #006600;\">LDAP</span><span style=\"color: #339933;\">;</span>\n<span style=\"color:
  #000000; font-weight: bold;\">use</span> Data<span style=\"color: #339933;\">::</span><span
  style=\"color: #006600;\">Dumper</span><span style=\"color: #339933;\">;</span>\n&nbsp;\n<span
  style=\"color: #666666; font-style: italic;\"># Set all your custom configs here:</span>\n<span
  style=\"color: #b1b100;\">my</span> <span style=\"color: #0000ff;\">$ldaphostname</span><span
  style=\"color: #339933;\">=</span><span style=\"color: #ff0000;\">'ldap.example.int'</span><span
  style=\"color: #339933;\">;</span>\n<span style=\"color: #b1b100;\">my</span> <span
  style=\"color: #0000ff;\">$groupdn</span><span style=\"color: #339933;\">=</span><span
  style=\"color: #ff0000;\">'ou=People,ou=Groups,dc=example,dc=int'</span><span style=\"color:
  #339933;\">;</span>\n<span style=\"color: #b1b100;\">my</span> <span style=\"color:
  #0000ff;\">$peopledn</span><span style=\"color: #339933;\">=</span><span style=\"color:
  #ff0000;\">'ou=People,ou=active,ou=Users,dc=example,dc=int'</span><span style=\"color:
  #339933;\">;</span>\n<span style=\"color: #b1b100;\">my</span> <span style=\"color:
  #0000ff;\">$contactdir</span><span style=\"color: #339933;\">=</span><span style=\"color:
  #ff0000;\">'/etc/nagios/contacts'</span><span style=\"color: #339933;\">;</span>\n&nbsp;\n<span
  style=\"color: #b1b100;\">my</span> <span style=\"color: #0000ff;\">$ldap</span>
  <span style=\"color: #339933;\">=</span> <span style=\"color: #000000; font-weight:
  bold;\">new</span> Net<span style=\"color: #339933;\">::</span><span style=\"color:
  #006600;\">LDAP</span> <span style=\"color: #009900;\">&#40;</span><span style=\"color:
  #0000ff;\">$ldaphostname</span><span style=\"color: #009900;\">&#41;</span><span
  style=\"color: #339933;\">;</span>\n<span style=\"color: #0000ff;\">$ldap</span><span
  style=\"color: #339933;\">-&gt;</span><span style=\"color: #000066;\">bind</span><span
  style=\"color: #009900;\">&#40;</span><span style=\"color: #009900;\">&#41;</span>
  <span style=\"color: #339933;\">||</span> <span style=\"color: #000066;\">die</span>
  <span style=\"color: #ff0000;\">&quot;failed to bind&quot;</span><span style=\"color:
  #339933;\">;</span>\n&nbsp;\n<span style=\"color: #666666; font-style: italic;\">#search
  for groups</span>\n<span style=\"color: #b1b100;\">my</span> <span style=\"color:
  #0000ff;\">$mesg</span> <span style=\"color: #339933;\">=</span> <span style=\"color:
  #0000ff;\">$ldap</span><span style=\"color: #339933;\">-&gt;</span><span style=\"color:
  #006600;\">search</span><span style=\"color: #009900;\">&#40;</span>\n        base
  <span style=\"color: #339933;\">=&gt;</span> <span style=\"color: #0000ff;\">$groupdn</span><span
  style=\"color: #339933;\">,</span>\n        filter <span style=\"color: #339933;\">=&gt;</span>
  <span style=\"color: #ff0000;\">&quot;(objectclass=posixgroup)&quot;</span><span
  style=\"color: #339933;\">,</span>\n        attrs <span style=\"color: #339933;\">=&gt;</span>
  <span style=\"color: #009900;\">&#91;</span><span style=\"color: #ff0000;\">'cn'</span><span
  style=\"color: #339933;\">,</span><span style=\"color: #ff0000;\">'memberuid'</span><span
  style=\"color: #009900;\">&#93;</span><span style=\"color: #339933;\">,</span>\n
  \       <span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n&nbsp;\n<span
  style=\"color: #666666; font-style: italic;\">#loop through each entry</span>\n<span
  style=\"color: #b1b100;\">while</span> <span style=\"color: #009900;\">&#40;</span><span
  style=\"color: #b1b100;\">my</span> <span style=\"color: #0000ff;\">$group</span><span
  style=\"color: #339933;\">=</span> <span style=\"color: #0000ff;\">$mesg</span><span
  style=\"color: #339933;\">-&gt;</span><span style=\"color: #006600;\">pop_entry</span><span
  style=\"color: #009900;\">&#40;</span><span style=\"color: #009900;\">&#41;</span>
  <span style=\"color: #009900;\">&#41;</span><span style=\"color: #009900;\">&#123;</span>\n&nbsp;\n
  \   <span style=\"color: #666666; font-style: italic;\">#set the name of the group</span>\n
  \   <span style=\"color: #b1b100;\">my</span> <span style=\"color: #0000ff;\">$cn</span><span
  style=\"color: #339933;\">=</span><span style=\"color: #0000ff;\">$group</span><span
  style=\"color: #339933;\">-&gt;</span><span style=\"color: #006600;\">get_value</span><span
  style=\"color: #009900;\">&#40;</span><span style=\"color: #ff0000;\">'cn'</span><span
  style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n&nbsp;\n
  \   <span style=\"color: #666666; font-style: italic;\">#create a comma-separated
  list of group members</span>\n    <span style=\"color: #b1b100;\">my</span> <span
  style=\"color: #0000ff;\">$members</span><span style=\"color: #339933;\">=</span><span
  style=\"color: #0000ff;\">$group</span><span style=\"color: #339933;\">-&gt;</span><span
  style=\"color: #006600;\">get_value</span><span style=\"color: #009900;\">&#40;</span><span
  style=\"color: #ff0000;\">'memberuid'</span><span style=\"color: #339933;\">,</span>
  asref <span style=\"color: #339933;\">=&gt;</span> <span style=\"color: #cc66cc;\">1</span><span
  style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n
  \   <span style=\"color: #0000ff;\">$members</span><span style=\"color: #339933;\">=</span>
  <span style=\"color: #000066;\">join</span><span style=\"color: #009900;\">&#40;</span>
  <span style=\"color: #ff0000;\">','</span><span style=\"color: #339933;\">,</span>
  <span style=\"color: #0000ff;\">@$members</span><span style=\"color: #009900;\">&#41;</span><span
  style=\"color: #339933;\">;</span>\n&nbsp;\n    <span style=\"color: #666666; font-style:
  italic;\"># create our configuration block</span>\n    <span style=\"color: #b1b100;\">my</span>
  <span style=\"color: #0000ff;\">$contactinfo</span><span style=\"color: #339933;\">=</span><span
  style=\"color: #ff0000;\">&quot;\n        # We only have one contact in this simple
  configuration file,\n        # so there is no need to create more than one contact
  group.\n&nbsp;\n        define contactgroup{\n            contactgroup_name       $cn\n
  \           alias                   $cn\n            members                 $members\n
  \           }<span style=\"color: #000099; font-weight: bold;\">\\n</span>&quot;</span><span
  style=\"color: #339933;\">;</span>\n&nbsp;\n    <span style=\"color: #666666; font-style:
  italic;\"># write this block to a file named after the group cn</span>\n    <span
  style=\"color: #000066;\">print</span> <span style=\"color: #ff0000;\">`echo &quot;$contactinfo&quot;
  &gt; $contactdir/group_$cn.cfg`</span><span style=\"color: #339933;\">;</span>\n<span
  style=\"color: #009900;\">&#125;</span>\n&nbsp;\n<span style=\"color: #666666; font-style:
  italic;\">#search for users</span>\n<span style=\"color: #b1b100;\">my</span> <span
  style=\"color: #0000ff;\">$mesg</span> <span style=\"color: #339933;\">=</span>
  <span style=\"color: #0000ff;\">$ldap</span><span style=\"color: #339933;\">-&gt;</span><span
  style=\"color: #006600;\">search</span><span style=\"color: #009900;\">&#40;</span>\n
  \           base <span style=\"color: #339933;\">=&gt;</span> <span style=\"color:
  #0000ff;\">$peopledn</span><span style=\"color: #339933;\">,</span>\n            filter
  <span style=\"color: #339933;\">=&gt;</span> <span style=\"color: #ff0000;\">&quot;(objectclass=person)&quot;</span><span
  style=\"color: #339933;\">,</span>\n            attrs <span style=\"color: #339933;\">=&gt;</span>
  <span style=\"color: #009900;\">&#91;</span><span style=\"color: #ff0000;\">'uid'</span><span
  style=\"color: #339933;\">,</span><span style=\"color: #ff0000;\">'cn'</span><span
  style=\"color: #339933;\">,</span><span style=\"color: #ff0000;\">'mail'</span><span
  style=\"color: #339933;\">,</span><span style=\"color: #ff0000;\">'description'</span><span
  style=\"color: #009900;\">&#93;</span><span style=\"color: #339933;\">,</span>\n
  \          <span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n&nbsp;\n<span
  style=\"color: #666666; font-style: italic;\">#loop through each entry</span>\n<span
  style=\"color: #b1b100;\">while</span> <span style=\"color: #009900;\">&#40;</span><span
  style=\"color: #b1b100;\">my</span> <span style=\"color: #0000ff;\">$user</span><span
  style=\"color: #339933;\">=</span> <span style=\"color: #0000ff;\">$mesg</span><span
  style=\"color: #339933;\">-&gt;</span><span style=\"color: #006600;\">pop_entry</span><span
  style=\"color: #009900;\">&#40;</span><span style=\"color: #009900;\">&#41;</span>
  <span style=\"color: #009900;\">&#41;</span><span style=\"color: #009900;\">&#123;</span>\n&nbsp;\n
  \   <span style=\"color: #b1b100;\">my</span> <span style=\"color: #009900;\">&#40;</span><span
  style=\"color: #0000ff;\">$pager</span><span style=\"color: #339933;\">,</span><span
  style=\"color: #0000ff;\">$addr</span><span style=\"color: #009900;\">&#41;</span><span
  style=\"color: #339933;\">;</span>\n    <span style=\"color: #666666; font-style:
  italic;\"># grab the four fields we're interested in</span>\n    <span style=\"color:
  #b1b100;\">my</span> <span style=\"color: #0000ff;\">$uid</span><span style=\"color:
  #339933;\">=</span><span style=\"color: #0000ff;\">$user</span><span style=\"color:
  #339933;\">-&gt;</span><span style=\"color: #006600;\">get_value</span><span style=\"color:
  #009900;\">&#40;</span><span style=\"color: #ff0000;\">'uid'</span><span style=\"color:
  #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n    <span style=\"color:
  #b1b100;\">my</span> <span style=\"color: #0000ff;\">$cn</span><span style=\"color:
  #339933;\">=</span><span style=\"color: #0000ff;\">$user</span><span style=\"color:
  #339933;\">-&gt;</span><span style=\"color: #006600;\">get_value</span><span style=\"color:
  #009900;\">&#40;</span><span style=\"color: #ff0000;\">'cn'</span><span style=\"color:
  #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n    <span style=\"color:
  #b1b100;\">my</span> <span style=\"color: #0000ff;\">$mail</span><span style=\"color:
  #339933;\">=</span><span style=\"color: #0000ff;\">$user</span><span style=\"color:
  #339933;\">-&gt;</span><span style=\"color: #006600;\">get_value</span><span style=\"color:
  #009900;\">&#40;</span><span style=\"color: #ff0000;\">'mail'</span><span style=\"color:
  #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n    <span style=\"color:
  #b1b100;\">my</span> <span style=\"color: #0000ff;\">$desc</span><span style=\"color:
  #339933;\">=</span><span style=\"color: #0000ff;\">$user</span><span style=\"color:
  #339933;\">-&gt;</span><span style=\"color: #006600;\">get_value</span><span style=\"color:
  #009900;\">&#40;</span><span style=\"color: #ff0000;\">'description'</span><span
  style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n&nbsp;\n<span
  style=\"color: #666666; font-style: italic;\"># Note that I store the user's SMS
  address and Instant Messenger</span>\n<span style=\"color: #666666; font-style:
  italic;\"># addresses in the description field in the following format:</span>\n<span
  style=\"color: #666666; font-style: italic;\"># SMS:1234445555@some.url;</span>\n<span
  style=\"color: #666666; font-style: italic;\"># IM:morgajel@yahoo,morgajel@gtalk;</span>\n<span
  style=\"color: #666666; font-style: italic;\"># for those interested, you can send
  email to an sms address </span>\n<span style=\"color: #666666; font-style: italic;\">#
  with help from &lt;a href=&quot;http://sms411.net/how-to-send-email-to-a-phone&quot;&gt;http://sms411.net/how-to-send-email-to-a-phone&lt;/a&gt;</span>\n
  \   <span style=\"color: #666666; font-style: italic;\">#strip pager info out of
  desc and format it</span>\n    <span style=\"color: #666666; font-style: italic;\">#(note
  it's in sms format):</span>\n    <span style=\"color: #b1b100;\">if</span> <span
  style=\"color: #009900;\">&#40;</span><span style=\"color: #0000ff;\">$desc</span>
  <span style=\"color: #339933;\">=~/</span>SMS<span style=\"color: #339933;\">:</span><span
  style=\"color: #009900;\">&#40;</span><span style=\"color: #009900;\">&#91;</span><span
  style=\"color: #339933;\">^;</span><span style=\"color: #009900;\">&#93;</span><span
  style=\"color: #339933;\">+</span><span style=\"color: #009900;\">&#41;</span><span
  style=\"color: #339933;\">;?/</span><span style=\"color: #009900;\">&#41;</span><span
  style=\"color: #009900;\">&#123;</span>\n        <span style=\"color: #0000ff;\">$pager</span><span
  style=\"color: #339933;\">=</span> <span style=\"color: #ff0000;\">&quot;        pager
  \          $1<span style=\"color: #000099; font-weight: bold;\">\\n</span>&quot;</span><span
  style=\"color: #339933;\">;</span>\n    <span style=\"color: #009900;\">&#125;</span>\n
  \   <span style=\"color: #666666; font-style: italic;\"># strip addresses out of
  desc and format them:</span>\n    <span style=\"color: #b1b100;\">if</span> <span
  style=\"color: #009900;\">&#40;</span><span style=\"color: #0000ff;\">$desc</span>
  <span style=\"color: #339933;\">=~/</span>IM<span style=\"color: #339933;\">:</span><span
  style=\"color: #009900;\">&#40;</span><span style=\"color: #009900;\">&#91;</span><span
  style=\"color: #339933;\">^;</span><span style=\"color: #009900;\">&#93;</span><span
  style=\"color: #339933;\">+</span><span style=\"color: #009900;\">&#41;</span><span
  style=\"color: #339933;\">;?/</span><span style=\"color: #009900;\">&#41;</span><span
  style=\"color: #009900;\">&#123;</span>\n        <span style=\"color: #b1b100;\">my</span>
  <span style=\"color: #0000ff;\">@multiple_addr</span><span style=\"color: #339933;\">=</span><span
  style=\"color: #000066;\">split</span><span style=\"color: #009900;\">&#40;</span><span
  style=\"color: #009966; font-style: italic;\">/,/</span><span style=\"color: #339933;\">,</span><span
  style=\"color: #0000ff;\">$1</span><span style=\"color: #009900;\">&#41;</span><span
  style=\"color: #339933;\">;</span>\n        <span style=\"color: #b1b100;\">my</span>
  <span style=\"color: #0000ff;\">$count</span><span style=\"color: #339933;\">=</span><span
  style=\"color: #cc66cc;\">1</span><span style=\"color: #339933;\">;</span>\n        <span
  style=\"color: #666666; font-style: italic;\"># break each address out into an addressX
  line</span>\n        <span style=\"color: #666666; font-style: italic;\"># (for
  informational purposes);</span>\n        <span style=\"color: #b1b100;\">foreach</span>
  <span style=\"color: #b1b100;\">my</span> <span style=\"color: #0000ff;\">$addr</span>
  <span style=\"color: #009900;\">&#40;</span><span style=\"color: #0000ff;\">@multiple_addr</span><span
  style=\"color: #009900;\">&#41;</span><span style=\"color: #009900;\">&#123;</span>\n
  \           <span style=\"color: #0000ff;\">$addr</span><span style=\"color: #339933;\">=</span><span
  style=\"color: #0000ff;\">$addr</span><span style=\"color: #339933;\">.</span><span
  style=\"color: #ff0000;\">&quot;        address$count        $addr<span style=\"color:
  #000099; font-weight: bold;\">\\n</span>&quot;</span><span style=\"color: #339933;\">;</span>\n
  \           <span style=\"color: #0000ff;\">$count</span><span style=\"color: #339933;\">=</span><span
  style=\"color: #0000ff;\">$count</span><span style=\"color: #339933;\">+</span><span
  style=\"color: #cc66cc;\">1</span> <span style=\"color: #339933;\">;</span>\n        <span
  style=\"color: #009900;\">&#125;</span>\n    <span style=\"color: #009900;\">&#125;</span>\n&nbsp;\n
  \   <span style=\"color: #666666; font-style: italic;\">#generate the first half
  of the contact info</span>\n    <span style=\"color: #b1b100;\">my</span> <span
  style=\"color: #0000ff;\">$contactinfo</span><span style=\"color: #339933;\">=</span><span
  style=\"color: #ff0000;\">&quot;\n    define contact{\n        contact_name    $uid
  \             ; Short name of user\n        use             generic-contact\n        alias
  \          $cn               ; Full name of user\n        email           $mail\n$pager\n$addr\n
  \   }&quot;</span><span style=\"color: #339933;\">;</span>\n&nbsp;\n    <span style=\"color:
  #666666; font-style: italic;\"># write this block to a file named after the user
  uid</span>\n    <span style=\"color: #ff0000;\">`echo &quot;$contactinfo&quot; &gt;
  $contactdir/$uid.cfg`</span><span style=\"color: #339933;\">;</span>\n<span style=\"color:
  #009900;\">&#125;</span></pre></td></tr></table><p class=\"theCode\" style=\"display:none;\">\r\n#!/usr/bin/perl\r\n#
  read your ldap repo and generation nagios config files for contacts\r\n# and contact_groups\r\nuse
  strict;\r\nuse Net::LDAP;\r\nuse Data::Dumper;\r\n\r\n# Set all your custom configs
  here:\r\nmy $ldaphostname='ldap.example.int';\r\nmy $groupdn='ou=People,ou=Groups,dc=example,dc=int';\r\nmy
  $peopledn='ou=People,ou=active,ou=Users,dc=example,dc=int';\r\nmy $contactdir='/etc/nagios/contacts';\r\n\r\nmy
  $ldap = new Net::LDAP ($ldaphostname);\r\n$ldap-&gt;bind() || die &quot;failed to
  bind&quot;;\r\n\r\n#search for groups\r\nmy $mesg = $ldap-&gt;search(\r\n        base
  =&gt; $groupdn,\r\n        filter =&gt; &quot;(objectclass=posixgroup)&quot;,\r\n
  \       attrs =&gt; ['cn','memberuid'],\r\n        );\r\n\r\n#loop through each
  entry\r\nwhile (my $group= $mesg-&gt;pop_entry() ){\r\n\r\n    #set the name of
  the group\r\n    my $cn=$group-&gt;get_value('cn');\r\n\r\n    #create a comma-separated
  list of group members\r\n    my $members=$group-&gt;get_value('memberuid', asref
  =&gt; 1);\r\n    $members= join( ',', @$members);\r\n\r\n    # create our configuration
  block\r\n    my $contactinfo=&quot;\r\n        # We only have one contact in this
  simple configuration file,\r\n        # so there is no need to create more than
  one contact group.\r\n\r\n        define contactgroup{\r\n            contactgroup_name
  \      $cn\r\n            alias                   $cn\r\n            members                 $members\r\n
  \           }\\n&quot;;\r\n\r\n    # write this block to a file named after the
  group cn\r\n    print `echo &quot;$contactinfo&quot; &gt; $contactdir/group_$cn.cfg`;\r\n}\r\n\r\n#search
  for users\r\nmy $mesg = $ldap-&gt;search(\r\n            base =&gt; $peopledn,\r\n
  \           filter =&gt; &quot;(objectclass=person)&quot;,\r\n            attrs
  =&gt; ['uid','cn','mail','description'],\r\n           );\r\n\r\n#loop through each
  entry\r\nwhile (my $user= $mesg-&gt;pop_entry() ){\r\n\r\n    my ($pager,$addr);\r\n
  \   # grab the four fields we're interested in\r\n    my $uid=$user-&gt;get_value('uid');\r\n
  \   my $cn=$user-&gt;get_value('cn');\r\n    my $mail=$user-&gt;get_value('mail');\r\n
  \   my $desc=$user-&gt;get_value('description');\r\n\r\n# Note that I store the
  user's SMS address and Instant Messenger\r\n# addresses in the description field
  in the following format:\r\n# SMS:1234445555@some.url;\r\n# IM:morgajel@yahoo,morgajel@gtalk;\r\n#
  for those interested, you can send email to an sms address \r\n# with help from
  &lt;a href=&quot;http://sms411.net/how-to-send-email-to-a-phone&quot;&gt;http://sms411.net/how-to-send-email-to-a-phone&lt;/a&gt;\r\n
  \   #strip pager info out of desc and format it\r\n    #(note it's in sms format):\r\n
  \   if ($desc =~/SMS:([^;]+);?/){\r\n        $pager= &quot;        pager           $1\\n&quot;;\r\n
  \   }\r\n    # strip addresses out of desc and format them:\r\n    if ($desc =~/IM:([^;]+);?/){\r\n
  \       my @multiple_addr=split(/,/,$1);\r\n        my $count=1;\r\n        # break
  each address out into an addressX line\r\n        # (for informational purposes);\r\n
  \       foreach my $addr (@multiple_addr){\r\n            $addr=$addr.&quot;        address$count
  \       $addr\\n&quot;;\r\n            $count=$count+1 ;\r\n        }\r\n    }\r\n\r\n
  \   #generate the first half of the contact info\r\n    my $contactinfo=&quot;\r\n
  \   define contact{\r\n        contact_name    $uid              ; Short name of
  user\r\n        use             generic-contact\r\n        alias           $cn               ;
  Full name of user\r\n        email           $mail\r\n$pager\r\n$addr\r\n    }&quot;;\r\n\r\n
  \   # write this block to a file named after the user uid\r\n    `echo &quot;$contactinfo&quot;
  &gt; $contactdir/$uid.cfg`;\r\n}</p></div>\n\";}"
---

The Following is an ugly little perl script I’ve whipped up for generating contact entries for Nagios; each file is read via a cfg\_dir command in the nagios.cfg. This script is set to run nightly, capturing any new users and updating users and groups automagically.

Let me know if you find it useful.

```
<pre lang="perl">

#!/usr/bin/perl
# read your ldap repo and generation nagios config files for contacts
# and contact_groups
use strict;
use Net::LDAP;
use Data::Dumper;

# Set all your custom configs here:
my $ldaphostname='ldap.example.int';
my $groupdn='ou=People,ou=Groups,dc=example,dc=int';
my $peopledn='ou=People,ou=active,ou=Users,dc=example,dc=int';
my $contactdir='/etc/nagios/contacts';

my $ldap = new Net::LDAP ($ldaphostname);
$ldap->bind() || die "failed to bind";

#search for groups
my $mesg = $ldap->search(
        base => $groupdn,
        filter => "(objectclass=posixgroup)",
        attrs => ['cn','memberuid'],
        );

#loop through each entry
while (my $group= $mesg->pop_entry() ){

    #set the name of the group
    my $cn=$group->get_value('cn');

    #create a comma-separated list of group members
    my $members=$group->get_value('memberuid', asref => 1);
    $members= join( ',', @$members);

    # create our configuration block
    my $contactinfo="
        # We only have one contact in this simple configuration file,
        # so there is no need to create more than one contact group.

        define contactgroup{
            contactgroup_name       $cn
            alias                   $cn
            members                 $members
            }\n";

    # write this block to a file named after the group cn
    print `echo "$contactinfo" > $contactdir/group_$cn.cfg`;
}

#search for users
my $mesg = $ldap->search(
            base => $peopledn,
            filter => "(objectclass=person)",
            attrs => ['uid','cn','mail','description'],
           );

#loop through each entry
while (my $user= $mesg->pop_entry() ){

    my ($pager,$addr);
    # grab the four fields we're interested in
    my $uid=$user->get_value('uid');
    my $cn=$user->get_value('cn');
    my $mail=$user->get_value('mail');
    my $desc=$user->get_value('description');

# Note that I store the user's SMS address and Instant Messenger
# addresses in the description field in the following format:
# SMS:1234445555@some.url;
# IM:morgajel@yahoo,morgajel@gtalk;
# for those interested, you can send email to an sms address 
# with help from <a href="http://sms411.net/how-to-send-email-to-a-phone">http://sms411.net/how-to-send-email-to-a-phone</a>
    #strip pager info out of desc and format it
    #(note it's in sms format):
    if ($desc =~/SMS:([^;]+);?/){
        $pager= "        pager           $1\n";
    }
    # strip addresses out of desc and format them:
    if ($desc =~/IM:([^;]+);?/){
        my @multiple_addr=split(/,/,$1);
        my $count=1;
        # break each address out into an addressX line
        # (for informational purposes);
        foreach my $addr (@multiple_addr){
            $addr=$addr."        address$count        $addr\n";
            $count=$count+1 ;
        }
    }

    #generate the first half of the contact info
    my $contactinfo="
    define contact{
        contact_name    $uid              ; Short name of user
        use             generic-contact
        alias           $cn               ; Full name of user
        email           $mail
$pager
$addr
    }";

    # write this block to a file named after the user uid
    `echo "$contactinfo" > $contactdir/$uid.cfg`;
}
```