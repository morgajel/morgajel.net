---
author: Jesse Morgan
categories:
- Uncategorized
date: "2010-07-25T15:48:11Z"
guid: http://morgajel.net/?p=893
id: 893
title: 'Revisited: Epson Perfection v350 on Ubuntu'
url: /2010/07/25/893
views:
- "254"
---

Jackie wanted me to reconfigure her scanner (since she hasn’t used it since I reloaded her system a while back) and I remembered what a pain it was to configure. A quick google search turned up the [article I wrote three years ago](http://morgajel.net/2007/09/02/219), so I thought I’d do follow-up article to see what’s changed.

## Get the Drivers

Since my last installation, things have changed. a bit. Many different [distros are now supported](http://avasys.jp/eng/linux_driver/download/), so we don’t have to shoehorn the RPMs onto the system with Alien. Here are the packages I grabbed off their site:  
`<br></br>iscan-data_1.0.1-1_all.deb<br></br>iscan_2.25.0-1.ltdl7_i386.deb<br></br>iscan-plugin-gt-f700_2.1.0-3_i386.deb<br></br>`

Once you have them, you can install them along with sane and sane utils:  
`<br></br>sudo dpkg -i iscan_2.25.0-1.ltdl7_i386.deb iscan-data_1.0.1-1_all.deb iscan-plugin-gt-f700_2.1.0-3_i386.deb<br></br>sudo apt-get install sane sane-utils<br></br>`

## Edit the Configs

Edit /etc/sane.d/dll.conf, adding *epkowa* to the list.

Create /etc/udev/rules.d/45-libsane.rules, adding the following:  
`<br></br># Epson Perfection v350<br></br>SYSFS{idVendor}=="04b8", SYSFS{idProduct}=="012f", MODE="664", GROUP="scanner"<br></br>`  
I use the group “scanner” above; You could go the easy route and use a group that you’re already part of, such as “users” or “admin”, otherwise you’ll have to create the scanner group, add yourself to it, then log out and log back in.

## Finishing Up

Afterwards, connect your scanner and run the following command:  
`<br></br>scanimage -L<br></br>`

The scanner should fire up now, which means you can use your newly installed iscan app to scan pictures.