---
author: Jesse Morgan
categories:
- Uncategorized
date: "2012-12-25T00:41:04Z"
guid: http://morgajel.net/?p=1285
id: 1285
title: Beyond my Ability, Part 3
url: /2012/12/25/1285
---

As I continue to translate Amit’s actionscript to javascript, it is becoming increasingly obvious that I’m missing a crucial part of this codebase- the actual voronoi diagram. Since this is a level of math that is beyond my ability, I’m going to cheat and find another implementation. [Raymond Hill](http://www.raymondhill.net/voronoi/rhill-voronoi-demo1.php) has a demo and a nice MIT-licensed [library ](http://www.raymondhill.net/voronoi/rhill-voronoi-core.js)that may be exactly what I need.

After I examine the API, perhaps I can figure out how much the map.js will need to change to use that, if they’re even compatible. I also need to figure out which conceptual primitives I will need. In the meantime I’ve gone through most of the map.js and translated the[ skeleton of it to javascript](https://github.com/morgajel/CityGenerator/blob/master/map.js). It’s not very useful, but it’s something.