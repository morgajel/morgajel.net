---
author: Jesse Morgan
categories:
- Electronics
- Hobbies
- IRC
- Linux
- Open Source
- Rant
date: "2006-03-05T21:47:57Z"
guid: http://morgajel.net/?p=103
id: 103
title: 'This Week&#8217;s 10 Minutes of Hate: Linux Midi'
url: /2006/03/05/103
views:
- "53"
---

Midi- oh, how do I loathe thee? Let me count the ways…

I’ve never liked midi, it’s fabricated, boring, hollow existence bothers me whenever I hear it, yet I am currently at it’s mercy. Linux may be ahead of other operating systems in many respects, but for some reason, midi support seems to have been completely passed over.

One of my side projects right now is writing a CD. a useful tool for writing a CD is a Piano. Since I do not own/have room for a piano, I decided to purchase a midi controller to attach to my computer. After careful research, went down to Guitar Center and picked up the second cheapest one I could find- the M-Audio Oxygen. It only has 25 keys, but I figure, hey I only got 10 digits. Even if I was to get my toes into the act, I’d have 5 keys to spare!

So anyways, I honestly have been planning on getting one for almost a year. I got it home and found the joy of trying to get it to work with Linux. Since my onboard soundcard was a piece of crap, I had problems getting midi to work in general. I got fed up and went and spent ANOTHER $80 to pick up a new sound card- another purchase i’ve been planning on. I got an Creative Soundblaster Audigy 4, which I was told had reasonable recording capabilities, hardware midi sequencer and reasonable linux support…

“Reasonable” is a funny word- it’s completely subjective. If a dung beetle came up and offered you a plate and said “hey man, eat up, this is good shit,” You’d probably realize he meant well, but still decline the plate full of feces. Taste, much like beauty, is in the eye of the beholder. So back to my reasonable support. After a bit of battling, I was able to get regular playback working- as an added bonus, I can now turn down the bass on my subwoofer so the neighbors don’t hate me. Midi, however, is still out of my grasp.

“They” were partially right. There’s lots of documentation. on all the various versions of the audigy (except of course mine), on different distributions, some using alsa, some using oss, finding different problems, using different versions of everything. No one person has anything remotely close to my setup from what I can tell- Audigy 4 + Gentoo + M-Audio Oxygen midi controller + ALSA. In other words, hundreds of pages of crap to sort through and very little of it useful by itself. I don’t blame the linux community for this, I blame the lazy manufacturers who refuse to help and make us end up doing it for them. It sucks and it pisses me off.

So anyways, I have three goals-

1. get midi playback working, so I can listen to ff2.mid
2. get my keyboard working so can input into the program called “noteedit”
3. figure out what part, if any, this remote and IR tower have in this.

### Getting Midi to Work 

Here’s an idea of where I’m at now.

```

draccus ~ # aconnect -o
client 64: 'Audigy MPU-401 (UART)' [type=kernel]
    0 'Audigy MPU-401 (UART)'
   32 'Audigy MPU-401 #2'
client 65: 'Emu10k1 WaveTable' [type=kernel]
    0 'Emu10k1 Port 0  '
    1 'Emu10k1 Port 1  '
    2 'Emu10k1 Port 2  '
    3 'Emu10k1 Port 3  '

draccus ~ # aconnect -i
client 0: 'System' [type=kernel]
    0 'Timer           '
    1 'Announce        '
client 64: 'Audigy MPU-401 (UART)' [type=kernel]
    0 'Audigy MPU-401 (UART)'
   32 'Audigy MPU-401 #2'

draccus ~ # lsmod
Module                  Size  Used by
snd_seq_midi            6752  0
snd_emu10k1_synth       6912  0
snd_emux_synth         35840  1 snd_emu10k1_synth
snd_seq_virmidi         5952  1 snd_emux_synth
snd_seq_midi_emul       6592  1 snd_emux_synth
snd_pcm_oss            47264  0
snd_mixer_oss          16832  1 snd_pcm_oss
snd_seq_oss            33920  0
snd_seq_midi_event      5888  3 snd_seq_midi,snd_seq_virmidi,snd_seq_oss
snd_seq                49936  8 snd_seq_midi,snd_emux_synth,snd_seq_virmidi,snd_seq_midi_emul,snd_seq_oss,snd_seq_midi_event
snd_emu10k1           118500  1 snd_emu10k1_synth
snd_rawmidi            20704  3 snd_seq_midi,snd_seq_virmidi,snd_emu10k1
snd_seq_device          7244  7 snd_seq_midi,snd_emu10k1_synth,snd_emux_synth,snd_seq_oss,snd_seq,snd_emu10k1,snd_rawmidi
snd_ac97_codec         92320  1 snd_emu10k1
snd_pcm                80904  3 snd_pcm_oss,snd_emu10k1,snd_ac97_codec
snd_timer              21444  3 snd_seq,snd_emu10k1,snd_pcm
snd_ac97_bus            1792  1 snd_ac97_codec
snd_page_alloc          8456  2 snd_emu10k1,snd_pcm
snd_util_mem            3328  2 snd_emux_synth,snd_emu10k1
snd_hwdep               7328  2 snd_emux_synth,snd_emu10k1
snd                    50596  16 snd_seq_midi,snd_emux_synth,snd_seq_virmidi,snd_pcm_oss,snd_mixer_oss,snd_seq_oss,snd_seq_midi_event,snd_seq,snd_emu10k1,snd_rawmidi,snd_seq_device,snd_ac97_codec,snd_pcm,snd_timer,snd_util_mem,snd_hwdep
audio                  44608  0
quickcam               73316  0
videodev                7360  1 quickcam
wacom                  13632  0
nvidia               4084560  12
usb_midi               22148  0
soundcore               7648  3 snd,audio,usb_midi
usbhid                 31648  0

```

I’ve used afxload to load my sf2 soundbank. I believe this has worked correctly.

I can open kmid and play ff2.mid, but no sound comes out. the little virtual keyboards flicker and play, but no sound. I’ve doublechecked and the “synth” slider in kmix/alsamixer/etc is at full volume.

### Getting the midi Keyboard to Work

I figure this can wait till we get the rest of midi working, but I’ve found when it’s hooked up via usb, /dev/midi and /dev/midi00 will both output characters when catted and the keyboard is pressed. This means usb-linux is working, but it’s still not recognized by alsa. I believe I also have to load some type of firmware into it, but I’m a little fuzzy on that part.

```

draccus ~ # cat /dev/midi
ÃƒÂ¾ÃƒÂ¾ÃƒÂ¾ÃƒÂ¾ÃƒÂ¾ÃƒÂ¾ÃƒÂ¾ÃƒÂ¾;2;;B;;B;;?
```

###  IR Tower and Remote

The soundcard came with a remote and USB IR tower. the remote is a Creative RM-1500.  
One of the posts on this page about [ALSA, audigy and emu10k1](http://www.alsa-project.org/alsa-doc/doc-php/template.php?company=Creative+Labs&card=Sound+Blaster+Audigy+ES.&chip=emu10k2&module=emu10k1) seemed to imply midi wouldn’t work until the IR tower was fixed. Check out the ” Sunday, 29 February 2004″ post for more info.

If anyone has any questions/comments/help/ explicit directions, I’d be very greatful. I just checked my history in firefox and I have over 100 articles that I’ve read and 34 google searches. I’m running out of steam.

BTW, big thanks to the guys of #alsa and K\_F for helping me get as far as I got- if I seem frustrated, it’s because I am 🙂

## Update 1

Alright, so after letting my subconcious work on the problem last night, I’ve come up with a plan of attack. what’s really hurting me on this is a lack of knowledge revolving around my hardware and alsa, so I’m going to do a bit of reading up about alsa today. I’ve come up with the following troubleshooting goals:

Identify a path that I can troubleshoot- how can I make sure asfxload is working? How can I verify Synth volume is up? what comes next? what needs to be there?

I should note that Timidity is up and running, and I can use it to verify that yes, synth volume is reasonable.

I guess the next step is to verify that soundfonts are being loaded.