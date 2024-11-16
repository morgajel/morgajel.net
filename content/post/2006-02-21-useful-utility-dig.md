---
author: Jesse Morgan
categories:
- Uncategorized
- Utility
date: "2006-02-21T13:14:21Z"
guid: http://morgajel.net/?p=87
id: 87
title: 'Useful Utility: dig'
url: /2006/02/21/87
views:
- "38"
---

No, not the popular social new site, the dns utility. Dig stands for *Domain Information Groper* (get it, DIG?), which fills the same niche as nslookup. As a matter of fact, dig is the successor to nslookup.Â Unlike nslookup however, the primary use of dig is non-interactive mode (which makes it ideal for scripting). Dig can also read batch files for more advanced executions, although I’ve not used this functionality myself.

So why bother using dig when nslookup is ubiquitous and you’re already familiar with it? There are [ a few good reasons](http://forums11.itrc.hp.com/service/forums/questionanswer.do?admit=109447626+1278785379926+28353475&threadId=1046212), but here are the high points:

- nslookup is flawed (the link above goes into detail)
- nslookup is deprecated (it may not be included in future releases of Bind)
- dig will eventually replace it (so stay ahead of the curve)

## Using Dig

So supposing that’s good enough for you, how do you use dig?  
`<br></br>dig [name] [@dns.server] [type] [query options]<br></br>`

It’s really straight forward- usually something as simple as *dig morgajel.net mx* would be a normal use, occasionally throwing it a dns server when testing bind replication.

As with most command line tools, you can also pass it various flags, but we’ll get to those a bit later. The *name* should be obvious- the name of the host you’re inquiring about. The second parameter here is the *dns server*, denoted by an **@**. If you don’t specify the DNS server, it will use the *resolv.conf* file. The next parameter is the *type* of record you’re inquiring about- *mx, soa, a, cname, txt,* etc. You can also use *any* to see all matching records, which is useful for seeing the big picture.

Besides bare parameters and command line flags, you can also pass mutliple query options prefaced with a **+**. Most options can also be negated by adding *no*; for example *+multiline* becomes *+nomultiline*. The most useful of these query options (for me):

- **+short**: cuts the response down to the bare minimum
- **+multiline**: makes responses like *soa* human readable
- **+search**: uses search and domain directives from resolv.conf

## Flags

The dig command accepts multiple command line flags, but to be honest, **-x** is the only one I use on a regular basis (for reverse lookups). In either case, here’s a breakdown of some of the more useful flags:

- **-x &lt;0.0.0.0&gt;**: Designate reverse lookup for ip addresses. This is the only flag I really use.
- **-b &lt;0.0.0.0&gt;**: Sets the base interface to query from on multi-homed machines. Useful for testing connectivity and firewall rules.
- **-f &lt;filename&gt;**: Allows you to send a batch file of commands to run.
- **-p &lt;port&gt;**: Allows you to designate a nonstandard port.
- **-q &lt;name&gt;**: Explicitly sets the name to remove order ambiguity.
- **-t &lt;type&gt;**: Explicitly sets the type to remove order ambiguity.

## Examples

Dig is one of those commands that you need to see examples of to fully appreciate. Here’s some fun you can try:  
`<br></br>dig -x 127.0.0.1<br></br>dig literaryescapism.com<br></br>dig @ns1.gopedro.net www.morgajel.com<br></br>dig morgajel.com any<br></br>dig @ns1.gopedro.net morgajel.net any +noquestion +nocomments +nostats<br></br>dig literaryescapism.com soa +short<br></br>dig literaryescapism.com soa +multiline`

So, if you haven’t made yourself familiar with dig, you should- I use it 99% of the time when working with DNS requests. As usual, if you have any suggestions or comments, leave them below.