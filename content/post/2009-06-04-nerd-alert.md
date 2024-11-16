---
author: Jesse Morgan
categories:
- Uncategorized
date: "2009-06-04T22:53:27Z"
guid: http://morgajel.net/?p=501
id: 501
title: nerd alert
url: /2009/06/04/501
views:
- "73"
---

I pride myself on being a geek, but every once in a while I do something that can’t be described any way other than “Nerdish”. Today I think I crossed that line again.

1\) A while back I hooked up my old PC speakers (kinda nice) to my server in the basement, then streamed music through it. Well, this was fine and dandy for me, but jackie wanted “her” music.

2\) I set up a web interface with several radio stations that jackie could select from with HTML “radio” buttons. This worked great, but it still required a laptop to hit the web interface to change channels.

3\) A few years ago I bought a nice soundcard to attempt to record my guitar playing on my workstation- that never panned out, but the card came with an USB IR receiver and a dinky little remote for watching DVDs… well, I got the remote working in linux (on my server) with lircd. I then have irexec listening to the lircd socket for buttonpushes, and executing commands based on my config in /etc/lircrc which liiks like this:

`<br></br>begin<br></br>    remote = Creative_RM-1500<br></br>    button = Vol_Down<br></br>    prog = irexec<br></br>    config = amixer -c 0 -- sset Master playback -1dB+<br></br>    mode = order<br></br>end<br></br>begin<br></br>    remote = Creative_RM-1500<br></br>    button = Vol_Up<br></br>    prog = irexec<br></br>    config = amixer -c 0 -- sset Master playback 2dB+<br></br>    mode = order<br></br>end<br></br>begin<br></br>    remote = Creative_RM-1500<br></br>    button = Mute<br></br>    prog = irexec<br></br>    config = amixer -c 0 -- sset Master toggle<br></br>    mode = order<br></br>end`

The end result? it raises and lowers the volume and can mute the audio on the server… what’s next?

4\) get Next and Previous to change stations.

Oh, and it’s all running as an unprivileged user rather than root, which makes me feel warm and fuzzy.

btw, here’s my /etc/lircd.conf:

```

begin remote

  name  Creative_RM-1500
  bits           16     
  flags SPACE_ENC|CONST_LENGTH
  eps            30           
  aeps          100           
  header       9078  4671     
  one             0     0     
  zero            0     0     
  pre_data_bits   16          
  pre_data       0x8322
  gap          106974
  toggle_bit_mask 0x0
  min_repeat      4


      begin codes
          Power                    0x000000000000619E
          1                        0x0000000000008B74
          2                        0x0000000000008F70
          3                        0x000000000000906F
          4                        0x0000000000008A75
          5                        0x000000000000847B
          6                        0x0000000000007887
          7                        0x0000000000008976
          8                        0x000000000000837C
          9                        0x0000000000007788
          0                        0x000000000000807F
          CMSS                     0x000000000000718E
          EAX                      0x0000000000008C73
          Mute                     0x0000000000006E91
          Vol_Down                 0x000000000000639C
          Vol_Up                   0x000000000000629D
          Up                       0x0000000000007B84
          Left                     0x0000000000008778
          Ok                       0x000000000000817E
          Right                    0x000000000000758A
          Down                     0x0000000000008D72
          Return                   0x0000000000008E71
          Start                    0x0000000000008877
          Cancel                   0x0000000000007C83
          Rec                      0x000000000000738C
          Options                  0x000000000000827D
          Display                  0x0000000000007689
          Previous                 0x0000000000007F80
          Play                     0x0000000000007986
          Next                     0x0000000000007A85
          Slow                     0x0000000000007D82
          Stop                     0x000000000000857A
          Step                     0x0000000000007E81
      end codes

end remote

```