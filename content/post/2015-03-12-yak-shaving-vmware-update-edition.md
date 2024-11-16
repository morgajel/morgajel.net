---
author: Jesse Morgan
categories:
- Uncategorized
date: "2015-03-12T14:44:48Z"
guid: http://morgajel.net/?p=1651
id: 1651
title: 'Yak Shaving: VMWare Update Edition.'
url: /2015/03/12/1651
---

1. Review nessus report, see Samba needs patching.
2. Patch Samba.
3. While retesting, I notice ESX has a patch that needs implementing.
4. Find out they released 6.0 today. Rather than upgrading to 5.5.1 then 6.0, I look into upgrading directly to 6.0
5. While looking to implement that I research updatemanager, which I can’t use since I don’t have a windows server to install it on.
6. So I look at doing it manually, and find out that I need to upgrade vcenter first, since vcenter can’t manage esx hosts that are a higher version.
7. I find https://www.youtube.com/watch?v=QXOkUVhIOA8 which seems to be exactly what I need.
8. Spend half an hour looking for OVA file similar to what was used for 5.5. It does not exist.
9. identify vcenter appliance download for 6.0. Download 3 gig ISO. Not an exact match, but close.
10. mount ISO locally. Setup file says “vCenter Server Appliance installer cannot run on Linux. It must be run on Windows.”
11. Spend half an hour searching for the friggen OVA.
12. Someone clues me in that “vcsa/vmware-vcsa” on the iso is actually the OVA file. Copy that from readonly ISO to local disk, rename it as vmware-vsca-6.0.ova
13. go to vcenter server web client, navigate to datastore.
14. See coworker set off alarms on one datastore for being overused. Need to look into that later.
15. Find out that I need to install a browser plugin to upload a friggen file to their web interface.
16. Download plugin, install it.
17. plugin doesn’t appear, realize that it installed in the wrong place.
18. research how to uninstall the stupid plugin, then reinstall it to the right place. Still doesn’t show.
19. Someone suggests using OVAtool. I don’t even remember what that does or if it’ll even help me. I don’t know if it works on linux, if I can install it on my workstation, or where to even find it.
20. restart chrome; lose half of my tabs when the second window doesn’t reappear. plugin still doesn’t work.

Day two:

1. retry chrome plugin, it fails to be detected again.
2. research to find out that the plugin interface that VMware uses is deprecated, and their plugin only works with archaic versions of chrome
3. Give up, load windows VM
4. download the \*deprecated\* vsphere client for windows
5. attempt to install vsphere. Installer disappears.
6. try reinstalling, receive error that installer is in progress, then fails, then receive another failure message regarding .net 3.5
7. installer refuses to run because an installer is already running.
8. reboot windows
9. run installer, installer disappears. Task manager shows background process “windows modules installer worker” using 99% of my disk bandwidth. maybe it’s still working?
10. after 10 minutes, installer reappears. entire install takes 25 minutes.
11. Upload OVA to datastore1 so it can be deployed.
12. Attempt to deploy the OVA template through the web interface. Notified that “The CLient Integration Plugin must be installed to enable OVF functionality.” (note OVA and OVF are interchangeable at this point.)
13. Unable to COPY said text from web interface because hell, why not. Text is not selectable; maybe it’s an image?
14. Attempt to create a new virtual machine from OVA template. Datastores inaccessible.
15. Attempt to deploy OVA from windows client. Am unable to deploy from datastore (i.e. I must reupload 1.8 gig file again).
16. Upload template, click through menus and get a brand new “fill in the blank” screen that I don’t recall seeing before. Attempt to fill it out to the best of my ability.
17. Start Appliance, fails due to password needing to be reset. Web interface does not respond.
18. After 20 minutes of digging, I delete it.
19. start over, leaving the form empty.
20. Similar Message: “Root password is not set. vmdir.password is not set; aborting installation.” Web interface does not respond.
21. review my notes from the 5.5 install.
22. dry run installation of 5.5 OVA file; existing form has 5 fields.; Hostname is the only one really required.
23. 6.0 OVA has 46 fields; It is unclear how many are required. Perhaps all of them. 
    1. if host network mode is set to DHCP, ip address and host network prefix are not required. Default gateway, dns servers, and host identity don’t state if they are required.
    2. SSO Configuration talks about a directory password for replication partner. Is this the 5.5 instance I’m planning to mirror? I don’t think so- I’m pretending this is a stand-alone instance so I can follow the migration video later, which presumes the new instance is already installed but not configured. Setting temporary password for administrator.
    3. leaving the rest of the SSO configuration default
    4. leave database config set to Embedded.
    5. Setting root password in System Configuration (which is different than the Administrator account password set 3 steps ago).
    6. Leaving upgrade configuration blank.
    7. Leave networking properties blank
24. After finally getting the vcenter 6.0.0 installed, it turns out 6.0.0 no longer uses port 5480, as seen in the [first video](I%20find https://www.youtube.com/watch?v=QXOkUVhIOA8), which was the only one that came up when searching for upgrades yesterday.
25. Because, why would the upgrade process from 5.1 to 5.5 be the same as 5.5 to 6.0, right?
26. Start searching again, find [this video](https://www.youtube.com/watch?v=qHkLONNch8o) which appears to cover what I need.
27. I install the vmware client integration plugin from the ISO I downloaded previously on the windows VM (which is nearly out of space at this point).
28. run upgrader from ISO.
29. Walk through all the options and get to step 4 before getting the message: “vCenterServer FQDN vcenter.foo.com does not match DNS servers “localhost.localdom,localhost” and ip addresses “192.168.2.220” from VC certificate. Examine the VC certificate and make sure it is valid and point to vCenter Server FQDN.”
30. Which if I’m reading correctly, means that before I can upgrade, I have to install a properly signed certificate on 5.5 for…. I’m gonna guess the :5480 interface, which may be a totally different cert than the one for :9443.
31. research and find that I can circumvent this by setting the cert regeneration flag in the :5480 interface and rebooting vcenter.
32. try the upgrade tool again
33. realize 3 seconds after clicking OK that I just started a process of unknown length at 2pm on a friday.
34. process finishes at 3pm, warns me that my license is about to expire for vcenter(!)
35. re-enter license, am told it is no longer valid.
36. panic as I realize our license is for vcenter 5, not vcenter 6.
37. research and am told that it’s a simple upgrade procedure in the vmware portal to get a new license key for 6.0
38. go through motions, import new key, everything is awesome.

I owe a tremendous debt of gratitude towards the guys in #VMware on freenode. without their assistance, I’d probably be under my desk sobbing right now.

MONDAY: I’ll continue by upgrading the esx hosts. I’m sure it’ll go smoothly.