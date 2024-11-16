---
author: Jesse Morgan
categories:
- Uncategorized
date: "2012-12-23T18:41:14Z"
guid: http://morgajel.net/?p=1282
id: 1282
title: Beyond my Ability, Part 2
url: /2012/12/23/1282
---

So I have two ways to go about implementing this- read through Amit’s actionscript and try to implement it in java, or [read though his article](http://www-cs-students.stanford.edu/~amitp/game-programming/polygon-map-generation/) and randomly mishmash internet code together to get a working prototype. While it’s tempting to just swipe [other people’s code](http://www.raymondhill.net/voronoi/rhill-voronoi-core.js), that won’t help me understand how it works.

So, I guess the first part is to start with a simple [HTML5 page with a canvas.](http://morgajel.net/worldmap.html)

```

<html>
    <head>
        <script src="map.js">
        </script>
        <script>
            window.onload = function(){
                var canvas = document.getElementById("worldmap");
                var context = canvas.getContext("2d");
            };
        </script>
        <title> CityGenerator's Worldmap</title>
    </head>
    <body>
        <canvas id="worldmap" style="height:600px;width:800px;border:1px solid black;">
        </canvas>
    </body>
</html>
```

There we go. So pretty. The next step (I think) is to come up with an object to represent the map; While the canvas is pretty, it’s just output. Amit seems to have done exactly this with [Map.as](https://github.com/amitp/mapgen2/blob/master/Map.as). Do I try a full blown reimplementation of his code without understanding it, or do I aim for a skeleton that can output to the canvas, and the build up from there? For now, I think I’ll get more out of the latter. I’ll start with a simple [map.js](http://morgajel.net/map.js) and try to port over the basic functionality. I’m not sure how successful this will actually be seeing as how he uses external libraries, which I will either need to emulate (no chance) or find a similar js version (slim chance).

Adding to the complexity is my lack of js-fu; I feel like someone who kind of knows Spanish trying to translate a Portuguese document to find out where Vasco De Gama hid his gold.

I’m currently in the process of stepping through the actionscript and replacing individual methods with javascript, while at the same time comprehending how they work. It’s a lot of effort; I’ve been at it for well over an hour and I’m only 9% complete. I need to take a break.

</body></html>