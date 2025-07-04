---
title: "Adding a NeoPixel ring LED to an Octoprint webcam"
date: 2023-06-05T20:41:50Z
draft: true
toc: false
comments: false
categories:
- sideprojects
tags:
- Adafruit
- RaspberryPi
- NeoPixel
- 3DPrinting
---

I have a webcam on my 3d printer in an enclosure, but it's really dark. Let's add a light.
<!--more-->


## Hardware:
- NeoPixel 6812-8p-32mm (8 pixel, 32mm ring)
- Raspberry Pi 3B+ (2017)
- wires
- Female Dupont connectors (3 pin, 1 pin, 1 pin)
- JST SM connector (4 pin male and female connectors)

# Wiring the NeoPixel

I soldered a two-inch long 4-wire ribbon cable to the neopixel:
- Red (5v)
- Black (ground)
- Blue (Data In)
- Green (Data Out, unused)

I have those connected to a 4-pin male JST SM connector to make future usage easier if I ever decide to repurpose or modify it.


# Wiring the Adapter cable
I have a second, 18-inch 4-wire ribbon cable with the female JST SM connector on one end, and the female dupont connector on the other end.

in the 3 pin connector I have the Red wire, a blank space, then the Black wire.  The Green and Blue wires are on single-pin female dupont connectors.

# Attaching the cable to the Raspberry Pi

On the J8 Pinout I used [this diagram](https://www.pi4j.com/1.2/pins/model-3b-plus-rev1.html) to figure out what to use.

- Red wire connects to Pin 2
- Black wire connects to Pin 6
- Blue wire connects to Pin 18 (i.e. D18)
- Green wire is not connected (while I could remove this, it's in the middle of the ribbon and I don't want to mess with it.)

# The Code

I wanted to keep this short and sweet; there's no error checking, but it shouldn't break too easily.

``` python
#!/usr/bin/env python3
import board
import neopixel
import sys

LED_COUNT=8
PIN=board.D18
brightness = int(sys.argv[1])

pixels = neopixel.NeoPixel(pin=PIN, n=LED_COUNT, pixel_order=neopixel.RGB)

for pixel in range(0, LED_COUNT):
    pixels[pixel] = (brightness,brightness,brightness)
```
# The Raspberry Pi config.txt
you'll need to prep your raspberry Pi
in your /boot/config.txt, change the following lines:
```
dtparam=audio=off  #switched from on to off
```


# The Environment
This is where it gets a bit tricky.

```bash
ssh into the Raspberry Pi
sudo su -
apt-get update
apt-get upgrade -y
apt-get install python3.9-venv
python -m venv ~/venv
source ~/venv/bin/activate
pip install RPi.GPIO neopixel-plus

#enable the LEDs
venv/bin/python3 plus.py 64
# disable the LEDs
venv/bin/python3 plus.py 0
```

Now it's just a matter of telling octoprint to enable and disable when printing.
