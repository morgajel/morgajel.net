---
author: Jesse Morgan
categories:
- Uncategorized
date: "2012-12-23T15:00:22Z"
guid: http://morgajel.net/?p=1277
id: 1277
title: Beyond my Ability, Part 1
url: /2012/12/23/1277
---

I’ve been working on my [CityGenerator](http://morgajel.net/cgi-bin/citygenerator) off and on for years; [it’s finally reached a point](https://github.com/morgajel/CityGenerator) where I’m content with it’s output and I want to move to the next level. I’d like to start generating an entire continent, then populating it with my CityGenerator. So, step 1- figure out how to generate an awesome map. I’ve tried a couple of things, including my last [couple](http://morgajel.net/2012/12/14/1241) of[ posts](http://morgajel.net/2012/12/15/1249) on using gimp, but that won’t work for the city generator- I need something that can scale reasonably well.

So there are two parts to this problem; figuring out how to do it, and how to implement it. Fortunately, someone named Amit has [already figured out and documented it magnificently](http://www-cs-students.stanford.edu/~amitp/game-programming/polygon-map-generation/). While I think his method isn’t perfect, I completely understand the design decisions he makes and would probably take the same shortcuts he has taken (specifically ensuring that there are no local minima to ease the creation of lakes and rivers).

Amit is a smart guy that makes me feel very dumb. Reading through his full article makes my head spin. The main problem with [his awesome code](https://github.com/amitp/mapgen2) is that it’s written in flash/actionscript, which makes it semi-useless to me (I have no flash development tools nor an inclination to get tied up in a now-dying proprietary pile of poo). This leads directly into my next problem- How do I implement it?

Like Amit’s code, my best bet on this is to refrain from doing this on the server side- the perlin noise algorithm can be fairly hefty, and under load would crush my dinky server- it would be better to have the clients do the heavy lifting if possible. That leaves me with one viable option- javascript. A combination of javascript and html5 canvas should do the trick nicely (this is also what I use to generate my city flags).

So, with the proper tools and directions written in another language, all that’s left is to interpret Amit’s code and method, then implement it. Hopefully this will be the first article of many on my progress, and I won’t just give up in frustration.

Next: [Beyond my Ability, Part 2](http://morgajel.net/2012/12/23/1282)