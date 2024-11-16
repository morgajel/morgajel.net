---
author: Jesse Morgan
categories:
- Entertainment
- Utility
- Work
date: "2007-03-19T14:01:50Z"
guid: http://morgajel.net/2007/03/19/184/
id: 184
title: ldapifying an ubuntu server
url: /2007/03/19/184
views:
- "117"
---

I recently wrote a nice little script in ruby for ldapifying new ubuntu servers- all the server needs is a ssh key set up for root, the rest is cake…

```

jmorg@util3:~/base_configs# ./ldapify -h
Usage: ldapify --install hostname [$options]
       ldapify --check hostname
       ldapify --uninstall hostname
        --install hostname           hostname to ldapify(foo.pub.local)
        --uninstall hostname         removes ldapification from hostname
    -a, --access_group access_group  access_group that has access to hostname
        --no_group_dn                No access_group limitations- use with caution.
    -c, --clusters x,y,z             clusters in which hostname belongs
        --aliases x,y,z              other aliased hostnames for the host
        --ignore-home                don't mount/unmount home
    -v, --verbose                    enable verbosity
    -q, --quiet                      silence all unneeded messages
    -h, --help                       Show this message

jmorg@util3:~/base_configs# ./ldapify --install log1.pub.local -a devboxes
backing up sources.list...
updating sources.list...
updating package list...
adding nfs entry to /etc/fstab...
Complete.
Mounting home, please wait...
complete.
installing debconf-utils...
patching debconf selections...
installing libnss-ldap ...
symlinking ldap.conf...
copy ssl cert and ldap.conf...
complete.
No Access group was given, using admin_only by default.
backup nsswitch.conf and pam.d files...
complete.
install nsswitch.conf and pam.d files...
complete.
ldap requires the manager password:
please verify the manager password:
store manpass...
installing sudo-ldap...

jmorg@util3:~/base_configs# ./ldapify --uninstall log1.pub.local
restore nsswitch.conf and pam.d files...
complete.
remove ssl cert and ldap.conf...
complete.
removing nfs entry...
complete.
unmounting home...
complete.
removing debconf-utils, libnss-ldap and libpam-ldap ...
removing ldap.conf symlinks...
removing sudo-ldap, restoring sudo...
retore sources.list...
updating package list...
ldap requires the manager password:
please verify the manager password:
jmorg@util3:~/base_configs#
```

So what all does it do?

- Sets up ldap authentication of user accounts
- mounts the nfs-based home directory
- Sets up ldap-based sudo rules
- Creates a host entry in the ldap server
- Adds an entry in the ldap server for the distro’s cluster and ldapified hosts cluster
- Can completely revert back to the original state

This script takes about 2:45 to run (mostly due to the 120 seconds of waiting for the /home dir to mount), and saves roughly half an hours worth of work. It’s not very stable (pre-ldapified boxes cause it to freak out when trying to re-install/remove) , but it will be a lot of help as we move towards ubuntu as our standard distro.