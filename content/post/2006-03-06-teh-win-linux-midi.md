---
author: Jesse Morgan
categories:
- Uncategorized
date: "2006-03-06T08:12:48Z"
guid: http://morgajel.net/2006/03/06/104/
id: 104
title: 'Teh Win: Linux Midi'
url: /2006/03/06/104
views:
- "41"
---

Well, right before I ran out the door for work this morning, I managed to capture the ever-elusive “win”. not just any win, but “Teh Win.”

```

morgajel@draccus ~ $ aconnect -o
client 64: 'Audigy MPU-401 (UART)' [type=kernel]
    0 'Audigy MPU-401 (UART)'
   32 'Audigy MPU-401 #2'
client 65: 'Emu10k1 WaveTable' [type=kernel]
    0 'Emu10k1 Port 0  '
    1 'Emu10k1 Port 1  '
    2 'Emu10k1 Port 2  '
    3 'Emu10k1 Port 3  '
```

While there are 6 devices listed, the important one is “Emu10k1 Port 0”, otherwise known as 65:0  
You see, the other devices are clever ruses by a soundcard that refuses to be tamed.

Here is EXACTLY what I did.

1. I loaded firmware with asfxload –verbose=255 /mnt/dvd/Audio/Common/SFBank/CT4MGM.SF2 -D 65:0
2. I started kmid
3. I set kmid to use emu10k1 Port 0
4. I loaded ff2.mid and hit play.

two seconds later, the world was alive with the synthetic cello playing a song from my childhood.

Joyous days.

now, the sonfofabitch is, I’ve done this exact same process over 100 times with differing variables. Today’s differing variable? That SPECIFIC soundfont.

I’m going to double check tonight, but in my rush before work it seemed like the .bnk files that came with gentoo \*didn’t work\*.

I verified that /mnt/dvd/Audio/Common/Media/Sndfont/Tabla.SF2 works as well, but it sound like straight percussion.

Either way, Midi work- real midi from the soundcard wavetable.

Now, the Midi keyboard is all that stands in my way.