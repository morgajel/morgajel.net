---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Work
date: "2006-12-16T21:31:14Z"
guid: http://morgajel.net/2006/12/16/177/
id: 177
title: 'New OS: Kubuntu 6.10'
url: /2006/12/16/177
views:
- "50"
---

Ok, trying out Kubuntu on my new work laptop and I’m liking it quite a bit- the only problems I’ve had so far are with Hibernate (which I think is self- inflicted) and wireless stuff. I’ve figured out the wireless stuff and wanted to mention it for the people out there having the same troubles as I did. First up, a little info on my setup:

**Model:** IBM T42 (note, not the T42p, which is awesome, the crappier model)  
**OS:** Kubuntu 6.10  
**Wireless NIC:** Intel Corporation PRO/Wireless LAN 2100 3B Mini PCI Adapter (rev 04)  
**Wireless Driver:** ipw2100 1.3

## Wireless

There are a lot of choices for network setup, and since I use WPA2 with PSK, I’m going to document that.

in /etc/network/interfaces:  
` auto eth1<br></br> iface eth1 inet dhcp<br></br> wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf.local<br></br>`  
in /etc/wpa\_supplicant/wpa\_supplicant.conf.local  
` ctrl_interface=/var/run/wpa_supplicant<br></br> ctrl_interface_group=0<br></br> eapol_version=1<br></br> ap_scan=1<br></br> fast_reauth=1<br></br> #For WPA-PSK network<br></br> network={<br></br>        ssid="myssid"<br></br>        key_mgmt=WPA-PSK<br></br>        proto=WPA2<br></br>        pairwise=TKIP<br></br>        group=TKIP<br></br>        psk="my big secure passphrase"<br></br> }<br></br>`  
/etc/modprobe.d/ipw2100.modprobe  
` options ipw2100 disable=0<br></br>`  
and that was it- I spent time tinkering with all sorts of things, but I think this was all there was. If I get brave and re-do it next weekend, I’ll make sure to fully document the process.

## Hibernate

While trying to get wireless working, I noticed that hibernation stopped working- I’m not sure what I did, but I’m pretty sure it’s my fault- it WAS working originally.

**UPDATE**  
DOH, it helps if the swap partition is mounted so hibernation has someplace to store the data! it’s all working good now.