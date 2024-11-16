---
author: Jesse Morgan
categories:
- Uncategorized
date: "2017-05-09T22:20:35Z"
guid: http://morgajel.net/?p=1742
id: 1742
title: Monoprice Maker Select Plus Upgrades
url: /2017/05/09/1742
---

Because I don’t know when to stop, I’m going to start working on upgrades for my printer.

### 1. Filament Guide

Apparently one of the common problems is that slack in the filament can cause tangles- the best way to work around this is a filament guide. The first filament guide I printed was loose- too loose to use by itself. The second style just didn’t print properly, even trying to print it 2 different ways. I ended up using a command strip to stick the first one in place, and that seems to be working for the time being. Perhaps later I can modify the model and make it a little better fit.

### 2. Thumbwheels

Another common problem is that the all-metal thumbwheels will jiggle free over time, causing the bed to unlevel. The Solution is to use nylon locking nuts (nylock nuts) , but they’re so tiny you wouldn’t be able to adjust them- that’s where the 3d printed thumbwheels come in. The nylocks go on the underside of the printed thumbwheel, allowing better control and a more coarse texture than the metal thumbwheels. So far they’re working well.

### 3. Octoprint

While it’s not a direct mod, I printed a 3d case for a raspberry pi and loaded the pi with a custom OS called Octoprint. It controls the printer over USB so you’re not constantly inserting and removing sdcards. In addition, it gives you a nice web interface where you can upload your gcode files, track the print progress, and tweak configurations. It even lets you time-lapse control a pi camera to see the status and verify things haven’t went off the rails.

### 4. Allen Wrench and Scraper Hook Support

This is more of a utility modification than anything- with the 3d prints, you usually need to scrape the print off the bed when it’s complete, which means you have a standard scraper always laying around. This gives you a hook to store the scraper on, as well as slots to place the allen wrenches.

### 5. Fun Fan cooler

My original intention was to go with the Dii cooler, but after some investigation I came across the [fun fan](http://www.thingiverse.com/thing:2023906) cooler, which looks like an earwig’s behind. it has a few print flaws which I’m going to attempt to fix and re-release it on thingiverse. So far it’s greatly improved the quality of my prints. Update: My attempt to fix the model failed miserably. I still have a lot to learn about organic modelling.

### 6. Pi Cam Arm

I’ve found a decent arm/camera holster for my raspberry pi camera, which should allow me to create timelapse videos. I still don’t have a great base due to the short cable I’m working with, but that should be remedied tomorrow. In the mean time, here’s a video: https://goo.gl/photos/AiX6PCX5Z45nR1Bu8 This was my second print of the Earwig vent/ Fun Fan Cooler.

### 7. Glass Bed

My glass bed has arrived, but the thermal pad won’t be here until Saturday. Between now and then I’ll have to print clips.

### Future Plans

Right now I’m planning on the following upgrades:

1. Z braces. I saw the tower shake a surprising amount during quick y axis movements- Z braces basically add a hypotenuse to the intersecting structure of the printer. The ones I’m looking at will have levelling feet. Update: Unfortunately, these are for the maker select, not the select plus, so they won’t fit. I’ll need to design my own.
2. Metal Hotend with slotted block. Microswiss makes a nice hotend that supposedly works much better.
3. Hardened steel nozzle. Another Microswiss upgrade that’ll let me work with a wider array of materials and temperatures.
4. Machined lever and extruder plate. The existing level that holds the filament in place will warp over time- this one won’t.

Overall this has been an interesting diversion so far.