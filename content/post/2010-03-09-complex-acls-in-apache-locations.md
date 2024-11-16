---
author: Jesse Morgan
categories:
- Uncategorized
date: "2010-03-09T09:15:22Z"
guid: http://morgajel.net/?p=679
id: 679
title: Complex ACLs in Apache Locations
url: /2010/03/09/679
views:
- "176"
---

So the problem I’m having is with limiting LDAP users access to WebDAV directories; specifically, how do I keep devs from committing to the release branch. The setup is each of the large Projects (Project1, Project2) has a trunk and release branch; however some pesky devs try to ninja changes into the release branch, circumventing the entire process. That’s bad. The access should go like this (note this is a subset of the mess I’m dealing with):

1. Everyone can read and write all projects under / (/Project1, /Project2) EXCEPT: 
    1. Only a certain team of devs (and I) can write to /Project1/trunk.
    2. Only a certain team of qa can write to /Project1/branches/release.

Specifically, I should be able to commit to trunk but not release, however that doesn’t seem to be the case. Here’s an abbreviated version of my vhost:

```
<VirtualHost 10.0.0.5:443>
 blah blah blah snip...
<Location />
     DAV svn
     SVNParentPath /var/svn/
     SVNPathAuthz off
     AuthNameÂ  "SVN Access"
     AuthTypeÂ  Basic
     AuthLDAPUrlÂ Â Â Â  "ldap://ldap.example.int:389/ou=Users,dc=example,dc=int?uid"
     AuthBasicProvider ldap
     AuthzLDAPAuthoritative off
     AuthLDAPGroupAttributeÂ Â  "memberUid"
     <LimitExcept none>
         Require valid-user
     </LimitExcept>
 </Location>
```

```
 <Location /Project1/trunk>
     # Everyone can read, but only devs (and I) can change.
     <LimitExcept REPORT GET OPTIONS PROPFIND>
         Require ldap-group cn=devs,ou=Groups,dc=example,dc=int
         Require ldap-user morgajel
         Satisfy any
    </LimitExcept>
```

```
 </Location>
```

```
 <Location /Project1/branches/release>
    # Everyone can read, but only QA can change.
    <LimitExcept REPORT GET OPTIONS PROPFIND>
        Require ldap-group cn=qa,ou=Groups,dc=example,dc=int
        Satisfy any
    </LimitExcept>
 </Location>
```

```
</VirtualHost>
```

Any thoughts as to why I’m still able to write to release? and no, I’m not in the QA group; I suspect it has something to do with the Locations essentially being nested. Since we’re managing users and groups with LDAP, simple SVN ACLs won’t work, and I’m not really sure how to accomplish what I need to do.

Thoughts?

Side note: REPORT GET OPTIONS PROPFIND are the [only methods needed for read-only svn webdav access](http://svn.apache.org/repos/asf/subversion/trunk/notes/http-and-webdav/webdav-protocol). Fun fact, huh?

# UPDATE:

I was overthinking the situation- Apache config is not programming. There is no inheritance between locations. There is no nesting. Once you create a new location, you need to set up perms for that, so setting the base / with read/write for everyone, I then define sublocations

```
Â <Location /Project1/trunk>
Â Â Â  # If you want to write to trunk, you need to be one of the required people. You can still read it.
Â Â Â  <LimitExcept PROPFIND OPTIONS GET REPORT>
Â Â Â Â Â Â Â Â  Require ldap-group cn=devs,ou=People,ou=Groups,dc=mrm,dc=int
Â Â Â  </LimitExcept>
Â Â Â  <Limit PROPFIND OPTIONS GET REPORT>
Â Â Â Â Â Â Â Â  Require valid-user
Â Â Â  </Limit>
 </Location>
```

```
 <Location /Project1/branches/release>
    # Everyone can read, but only QA can change.
    <LimitExcept REPORT GET OPTIONS PROPFIND>
        Require ldap-group cn=qa,ou=Groups,dc=example,dc=int
        Satisfy any
    </LimitExcept>
Â Â Â  <Limit PROPFIND OPTIONS GET REPORT>
Â Â Â Â Â Â Â Â  Require valid-user
Â Â Â  </Limit>
Â </Location>
```

What I was missing was the second half of the limits, thinking it would inherit from /. It doesn’t.Â Without that require valid-user, it was allowing unauthenticated users to read the files (which was no good). Life is good (until I find out where this is broken).

# UPDATE 2:

So the above didn’t work when I retested it, so I tried one last time with good results (note the goals have changed, but the principles still apply):

```

<VirtualHost 10.0.0.5:443>
    SSLEngine on
    SSLCertificateFileÂ Â Â Â Â  /etc/pki/certs/example.int.crt
    SSLCertificateKeyFileÂ Â  /etc/pki/certs/example.int.key
    DocumentRootÂ Â Â Â Â  /var/www/svn.example.int/docs
    ServerNameÂ Â Â Â Â Â Â  svn.example.int
    ServerAliasÂ Â Â Â Â Â  svn.exampleco.com
    ErrorLog logs/svn.example.int-error_log
    CustomLog logs/svn.example.int-access_log common

    <Location />
        DAV svn
        SVNParentPath /var/svn/
        SVNPathAuthz off
        AuthNameÂ  "SVN Access"
        AuthTypeÂ  Basic
        AuthLDAPUrlÂ Â Â Â Â Â Â Â Â Â Â Â  "ldap://ldap.example.int:389    /ou=Users,dc=example,dc=int?uid"
        AuthBasicProvider ldap
        AuthzLDAPAuthoritative on
        AuthLDAPGroupAttributeÂ Â  "memberUid"
        AuthLDAPGroupAttributeIsDN off
        order Deny,Allow
        Deny from all
        Satisfy any
       
        # access
        <LimitExcept NONE>
            Require valid-user
            Satisfy any
        </LimitExcept>
    </Location>
   
   
    <Location /ProjectA/branches/releases//>
        order Deny,Allow
        Deny from all
        Satisfy any
   
        #read-only accessÂ Â  Â 
        <Limit GET PROPFIND OPTIONS REPORT>
            Require valid-user
            Satisfy any
        </Limit>
        # write access
        <LimitExcept GET PROPFIND OPTIONS REPORT>
            Require ldap-group cn=Administrators,ou=People,ou=Groups,dc=example,dc=int
            Require ldap-group cn=Team Leads,ou=People,ou=Groups,dc=example,dc=int
           #Â Â Â Â Â Â Â Â Â Â Â  Require ldap-user jmorgan
            Satisfy any
        </LimitExcept>
    </Location>
   
   
    <Location /ProjectB/branches/releases//>
        order Deny,Allow
        Deny from all
        Satisfy any
       
        #read-only accessÂ Â  Â 
        <Limit GET PROPFIND OPTIONS REPORT>
            Require valid-user
            Satisfy any
        </Limit>
        # write access
        <LimitExcept GET PROPFIND OPTIONS REPORT>
            Require ldap-group cn=Administrators,ou=People,ou=Groups,dc=example,dc=int
            Require ldap-group cn=Team Leads,ou=People,ou=Groups,dc=example,dc=int
           #            Require ldap-user jmorgan
            Satisfy any
        </LimitExcept>
    </Location>
   
    <Location /ProjectC/branches/releases//>
        order Deny,Allow
        Deny from all
        Satisfy any
       
        #read-only access    
        <Limit GET PROPFIND OPTIONS REPORT>
            Require valid-user
            Satisfy any
        </Limit>
        # write access
        <LimitExcept GET PROPFIND OPTIONS REPORT>
            Require ldap-group cn=Administrators,ou=People,ou=Groups,dc=example,dc=int
            Require ldap-group cn=Team Leads,ou=People,ou=Groups,dc=example,dc=int
           #            Require ldap-user jmorgan
            Satisfy any
        </LimitExcept>
    </Location>
   
</VirtualHost>
   
```

I am SURE this one has some redundancy, but quite honestly, I don’t want to deal with it anymore- I have far more important things.