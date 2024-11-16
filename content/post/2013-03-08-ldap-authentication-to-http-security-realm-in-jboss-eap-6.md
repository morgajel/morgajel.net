---
author: Jesse Morgan
categories:
- Uncategorized
date: "2013-03-08T13:37:46Z"
guid: http://morgajel.net/?p=1372
id: 1372
title: LDAP authentication to HTTP Security Realm in JBoss EAP 6
url: /2013/03/08/1372
---

So, you want to tie your jboss EAP 6 management interface into LDAP? Here’s how. This is for EAP 6 in Domain mode tying to an OpenLDAP server, but it should work for Standalone mode as well (I guess, I have no idea for sure).

Open up your jboss-eap/domain/configuration/host.xml and

1. Add a new security realm: ```
    <security-realm name="LDAPRealm">
        <authentication>
            <ldap connection="ldap_connection" base-dn="ou=users,ou=people,dc=example,dc=com">
                <username-filter attribute="uid"/>
            </ldap>
        </authentication>
    </security-realm>
    ```
2. In the same &lt;management&gt; block as &lt;security-realms&gt;, add ```
    <outbound-connections>
        <ldap name="ldap_connection" url="ldap://ldap.example.com" search-dn="cn=search,dc=example,dc=com" search-credential="secret"/>
    </outbound-connections>
    ```
3. and in your management-interfaces, change the realm from ManagementRealm to LDAPRealm: ```
    <management-interfaces>
        <native-interface security-realm="LDAPRealm">
            <socket interface="management" port="${jboss.management.native.port:9999}"/>
        </native-interface>
        <http-interface security-realm="LDAPRealm">
            <socket interface="management" port="${jboss.management.http.port:9990}"/>
        </http-interface>
    </management-interfaces>
    ```

and restart EAP. Some caveats:

1. <span style="line-height: 12.796875px;">JBoss is derpy and does not provide a way to anonymously bind for authentication, meaning the search-dn and search-credential are REQUIRED and you need to set up an inetOrgPerson or something to authenticate. It chokes if you leave either or both fields blank.</span>
2. This replaces the ManagementRealm that was previously used but does not remove it. If you lock yourself out, you can just revert step 3 to go back to using your hard-coded users.
3. Note the lack of TLS being mentioned. Does that mean it’s plaintext? it didn’t complain that my cert was self signed, so that’s entirely likely. Perhaps you can use a ldaps:// url and use the older ssl method if you have it set up. Very poor form in either case.

Let me know if this was any help.