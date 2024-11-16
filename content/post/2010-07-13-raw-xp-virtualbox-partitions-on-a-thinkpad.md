---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Work
date: "2010-07-13T12:50:30Z"
guid: http://morgajel.net/?p=879
id: 879
title: Raw WinXP Virtualbox Partitions on a Thinkpad
url: /2010/07/13/879
views:
- "196"
---

New job, new laptop. Many utilities here are windows only, so it requires a bit of… effort… to get myself up and running efficiently. The solution to the windows problem is VirtualBox. I had set this up on my last laptop with little effort, but this time around required a bit more effort. Hopefully the instructions below will help others get up and running quickly.

**Disclaimer**– your laptop may catch on fire and explode (or worse) if you attempt this… or something.

We’ll be presuming that you’ve already resized your windows partition and have both a working Windows and Linux partition.

## In Windows

Log into XP, grab [MergeIDE.zip](http://www.virtualbox.org/attachment/wiki/Migrate_Windows/MergeIDE.zip) from Virtualbox’s site, extract and run it. It should be a quick flash and be done. (Note: I am not 100% sure this step is needed)

Create a new hardware profile and name it **virtualbox**. Make sure to set it as a choice during boot. Try rebooting into native windows once to ensure that it does offer you profile options.

## In Linux

You’ll need the following packages installed (May differ for non-ubuntu systems):  
`mbr, virtualbox-ose, virtualbox-ose-qt`

Create a stand-alone mbr file to use for booting (yes, you need the force flag):

`install-mbr ~/.VirtualBox/WindowsXP.mbr --force`

We’re presuming that your windows partition is /dev/sda1. In the below command, we are defining

- a vmdk file (WindowsXP.vmdk)
- which raw disk to read (/dev/sda)
- which partition (1)
- the new MBR file we just created

`VBoxManage internalcommands createrawvmdk -filename ~/.VirtualBox/WindowsXP.vmdk -rawdisk /dev/sda -partitions 1 -mbr ~/.VirtualBox/WindowsXP.mbr -relative -register`

Note that you’ll need read/write access to that drive as your user, so you may want to figure out a cleaner/securer way to implement this, rather than adding your user to the disk group (which is very dumb and insecure). I would, but it’s working and I have more important things to do at the moment.

Another issue- apparently thinkpads report the [drive heads and cylinders oddly](http://ubuntuforums.org/showthread.php?p=6716355) (T410 for me and T60p in article), so we have to add some vmdk settings before virtualbox creates them incorrectly. Open ~/.VirtualBox/WindowsXP.vmdk and add the following at the bottom:  
`<br></br>ddb.geometry.biosCylinders="1024"<br></br>ddb.geometry.biosHeads="240"<br></br>ddb.geometry.biosSectors="63"<br></br>`  
The biosHeads appears to be the magic value- it seems to work if it’s set to 240, but the default is 255 (which fails).

Once you add those, start up virtualbox and check the virtual media manager, your new vmdk should be listed there. Once it’s confirmed, create a new virtual machine. Rather than creating a disk, select your vmdk as an existing disk.

After you finish, go the the VM settings-&gt;system and make sure the motherboard tab as io-apicÂ enabled (I also had PAE/NX enabled under processor and VT-x enabled under Acceleration).

## Start the VM

There are several errors that could pop up. I’m sure there are plenty more that I stumbled across, but these were the two big ones:

- **a disk read error occurred, press ctrl+alt+del to restart** – Caused by incorrect biosHeads- check and make sure it’s set to 240 (this was the fix for me, results may vary).
- **Complaint about kvm/vmx** – Virtualbox does not like kvm. Uninstall qemu-kvm.

If things go well, it should flicker mbr in the corner, then go to the hardware profile selection screen. Select the virtualbox profile, and continue, then log in.

What follows is a half-hour of installing generic drivers and dealing with hardware specific auto start apps complaining that they won’t work on this installation. Windows will warn that the new drivers are not blessed, so be forewarned.

Once completed, at the top of the VM windows select Devices-&gt; Install Guest Additions. This will download and mount an ISO, and windows will pop open a folder with the addition executables. Select the one best for you and run the installer. It will prompt you for video and mouse drivers (and trust me, you want them).

The final step is to shut down the windows VM, then reboot into the native windows partition to make sure it still works.Â I did receive a few blue-screens before logging in at the beginning, but they appeared random and haven’t happened since.

And that’s all there is to it- simple, eh? Your windows partition should now run in native mode and vm mode.