---
author: Jesse Morgan
categories:
- Uncategorized
date: "2006-03-07T08:20:24Z"
guid: http://morgajel.net/2006/03/07/106/
id: 106
title: M-Audio Oxygen (O2) Midi Controller
url: /2006/03/07/106
views:
- "41"
---

As I mentioned previously, I picked up a midi controller. I’ve recently gotten midi on my sound card to work. now I want to get this beast of a keyboard working before I chuck it out the window. Here’s my current setup:

- Running Gentoo
- Running 2.6.15-r1 kernel from gentoo-sources
- using alsa drivers that came with the kernel
- all other alsa packages are 1.0.10
- using http://alsa.opensrc.org/USBMidiDevices as a guide
- using version 1.2 of the usb midi firmware loader: http://sourceforge.net/projects/usb-midi-fw/

I seem to be hitting a rough spot when connecting the controller:

```

Mar  6 23:28:19 draccus kernel: midi: probe of 1-2.4:1.0 failed with error -5
```

that error: -5 scares me. I traced it back to an conflict between the legacy OSS usb\_midi and the alsa snd-usb-audio modules. I thought I needed both, since usb-midi makes /dev/midi work, and the articles are saying I need snd-usb-audio. It turns out they are mutually exclusive, or at least should be.

```

draccus ~ # aconnect -i
client 0: 'System' [type=kernel]
0 'Timer           '
1 'Announce        '
client 64: 'Audigy MPU-401 (UART)' [type=kernel]
0 'Audigy MPU-401 (UART)'
32 'Audigy MPU-401 #2'
client 72: 'USB O2' [type=kernel]
0 'USB O2 MIDI 1   '
```

See that last line? SUCCESS! But, we’re not working yet. I also found out after my latest reboot that asfxload no longer likes 65:0 as an output. and yes, it still shows up in aconnect with the same ID numbers. I don’t know if it’s related to a working midi keyboard or not. Whatever- the good news is it no longer needs the -D 65:0 to load soundbanks- I found this one out myself. Make sure you load it with -v 100 as well for full volume- this is independent of the slider in alsamixer, for some stupid-ass reason.

```

asfxload --verbose=255 \ 
        /mnt/dvd/Audio/Common/SFBank/CT4MGM.SF2   -v 100

```

now install **kaconnect** yeah, I know I’m springing an install on you at the last minute- I just don’t know how to do this any other way. using that, click in the USB input in the left column and the first emu10k1 wavetable entry (or whatever you’re using) and click connect.

Now press the Keyboard.

I got sound!  
only about 1/2 of the top controls work, so be forwarned that you’ll have to do a little digging. to get it operable. Once I get it all working, I’m gonna tear it apart and get it working on jackie’s computer with my soundcard.

but first, I still need to get it working in noteedit/rosegarden.