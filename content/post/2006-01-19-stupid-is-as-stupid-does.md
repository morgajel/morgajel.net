---
author: Jesse Morgan
categories:
- IRC
- Jackie
- Linux
- Open Source
- Rant
- Work
date: "2006-01-19T09:30:22Z"
guid: http://morgajel.net/?p=44
id: 44
title: Stupid is as Stupid does.
url: /2006/01/19/44
views:
- "45"
---

I don’t even know where to begin. This morning has sucked and it’s no ones fault but my own. It started with the DMA crap- the one machine where it would really help is the fileserver. I needed to simply change the chipset driver, recompile the kernel, and reboot. This should be simple for even a semi-competent linux users.

However, I am a moron. Each mistake I make, I’m gonna put a little \* next to it.

So, first thing I decide is “well, since I’m changing kernels, I might as well upgrade**\*\[1\]**“. so I grab the 2.6.14-r5 source, make oldconfig, and set it up all perfect. I add the lines to my grub config and sit on it for a few days before going through with it.

I notice that the grub config is rather old… like original configuration and note “that’s odd. I must not have known how to deal with scsi when I built this machine, and changed over to lilo. oh well, I know what I’m doing now**\*\[2\]**, so I’ll just install grub to the MBR”

flash forward to this morning. I’ve decided to reboot, and with an hour before work, I figure that’s more than enough time to troubleshoot if problems arise**\*\[3\]**.

I reboot…. notice it’s one of the older bios’ that counts ram. there 640 megs in this machine, so it takes a while… it also has a scsi controller, so it has to count through all of that. all in all, a very slow boot. This is important to remember- it takes 2 minutes of fricken waiting to get through the reboot process.

Nothing happens.

“eh? That’s not good.”

This by the way, is the worst thing you could ever possibly hear me utter. So I reboot. Two minutes later, it does the same thing.

“Crap, lemme just get a recovery CD out and fix it…. CRAP.”**\*\[4\]** The machine has 1 scsi drive with the OS, and 4 IDE HDs for storage- I took out the CDROM when I added the 200gig HD.

alright… ok, I see what happend… I told grub to check hd0 instead of hd4 for the root directory **\*\[5\]**– normally it would be hd0(the first ide device) but since I’m using scsi, it comes up as hd4.

“lemme try booting off of hda1 instead of the scsi drive…” well, I couldn’t remember what order they were installed in, and I didn’t want to really crack the case, so I tried booting ALL 4 IDE DRIVES, keeping in mind each try takes not only a 2 minute boot, but I can’t access the bios until it runs through the boot process and the scsi process, then a reset and boot with new settings. so each drive takes a minimum of 4 minutes, plus me mucking with the bios.

Well, nothing works- each one goes to a dead screen. “Well, I guess I can use a boot floppy…”**\*\[6\]\*\[7\]** I get a floppy from jackie, and start busting out my cdroms looking for a bootimage… Couldn’t use lynx because the fileserver is also the internal dhcp and dns server**\*\[8\]** couldn’t find one on the redhat CD I had… couldn’t find one on the gentoo CD I had… couldn’t find my suse Cds… couldn’t find a usable one on my ancient slackware CDs (copywrite 1995)… finally found one on a burned slackware CD from who knows when… probably 2002. find the img, dd it to the floppy, and pop the floppy in the fileserver.

I then start to think ahead**\*\[9\]** “well, if this doesn’t work, I better locate an extra CDROM and pull a HD.” So I boot the machine, change the boot order to take the floppy (another 4 minutes), find an extra CDROM that is of questionable repair, and begin to slide the side panel off.**\*\[10\]** The machine shuts down. I jiggle the power cable. I press the power button… nothing. The only thing I saw was the power button was blinking.

I freak out.**\*\[11\]** No if, ands or buts about it.

Jackie asks me “maybe the power supply fell off”. Now I know what she means- she meant two things- “maybe the power cable came loose”, and “maybe the power supply failed.” I know this and I love her very much, but at that moment in time she inspired a very deep and dark rage inside of me.

I took it out on everything that was laying on top of that case.**\*\[12\]** First I gently moved things like my ceramic change jar, and the foo-foo carpet cleaning powder that got set there they last time the carpet was vaccumed. Then I started throwing things: The nic card that I was meaning to put in the machine; the spare USB dongle; the cane of carpet cleaner that got put there the last time the cats yacked on the carpet.

Then I swept everything else off. Jackie was yelling at me at this point, something about calming down. I grabbed the side panel and threw it on the pile of shit on the floor. my hands were shaking. There was no fucking way this power supply just died on me. The inside of the case was covered in dust, so I sprayed it out with a can of air**\*\[13\]** and checked all the cables. everything is as it should be. There is no reason that the machine should have powered off when I opened the case….

then I saw the case sensor. it was an old server box, and I forgot it was there. I didn’t even know it worked. well, apparently when this latch is triggered, it powers off the machine. so I followed it, in the hope I could rip it from the mobo and boot back up… it lead to the front of the machine. so I had to peel off the front cover- there was a second button. with both buttons freed, the blinky light went off. I pulled the power cable to make sure everything was cycled correctly, reattached the power and it booted up fine. Finally, I can boot off the floppy and get this fixed….

Jackie left for work at this point- I mumbled an apology, knowing I was now late for work because I was gonna miss the bus. my one hour was up.

the machines boots, and starts to read the floppy….  
nothing.

“fuckit, I’m hooking up the CDROM.” I spend a few moments trying to read the jumper settings of the HD I’m trying to replace with a CDrom. it’s the primary slave. I set the CDROM accordingly. I jump through the bios hoops. I get to the CDROM command line.

I start to calm down. I fix the grub command lines to use hd4, and then attempt to install grub to /dev/sda (the scsi drive.)

Grub balks. I don’t know why, but it does not like hd4. I try for 10-15 minutes, looking in a lunux+ certification book for grub, I look in a fedora bible for hints, I look everwhere. I finally check my LPIC 1 Exam Cram book and determine this is total crap, I did everything perfectly- for some ungodly reason it doesn’t like my scsi drive.

“screw it, I’ll just use lilo.” I adjust the lilo.conf, reload lilo to /dev/sda (LILO didn’t have a problem with it) and shut it down. I disconnect the CDROM, put the face plate back on, power it up and wait…

Sweet, sweet lilo boot screen- how I’ve missed you.

I boot up using the new kernel and hold my breath. everything works. my workstation comes back to life (remember, my this was the fileserver I totalled- my /home was on it.)

I notice I have 10 minutes to get ready to go to catch the next bus. Grinning, I put the side cover back on… **\*\[14\]**

the machine dies. the power light blinks.

“AAAAAAAAAAAAAAAARRRRRRJGHH I HATE THIS MACHINE ”

I pull the power plug, remove the side panel, then remove the face plate. I press the power button a couple of times for good measure, reconnect everything and power it back up. It all comes up properly. I’m throwing on clothes and getting prepared while it’s booting. I haven’t combed my hair, I haven’t eaten, but I have no time now. I hit the lights, lock the door and run to catch the bus. I just barely made it- I got to work half an hour later than normal.

Now, I’ve counted about 14 or so stupid things here. Bonus points to the person who can name what I did wrong at each of those points.

(And yes VP, I know I you don’t know what a DMA is because you have a Mac.)