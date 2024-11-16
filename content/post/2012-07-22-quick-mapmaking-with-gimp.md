---
author: Jesse Morgan
categories:
- Uncategorized
date: "2012-07-22T22:57:50Z"
guid: http://morgajel.net/?p=1211
id: 1211
title: Quick Mapmaking with Gimp
url: /2012/07/22/1211
---

Ok, so here are some quick steps to make a simple map.

1. Create a small image, 20px by 20px
2. Drop solid black onto background layer
3. Create layer called “path”
4. With white 1px pen, drag a windy area
5. Upscale to 500px, turn off interpolation
6. select white area by color on Path layer
7. select distort – thresh 127, spread 8, granularity 2, smooth 2, smooth horizontal and vertical both checked
8. create new layer called floor
9. add layer mask (black)
10. fill in selected area with white on layermask
11. hide path layer
12. switch to layer mask of floor, select white
13. select-&gt;grow selected area by 20px
14. dropfill floor layer with white- this should reveal the layermask
15. filter -&gt; noise -&gt; rgb noise w/ rgb .6, alpha 0 no checkboxes
16. filter -&gt;distort -&gt;mosaic, oct+squares, size 5, height 1, neat .2, light direction 135, color variation .2 all checked
17. Gaussian blur 5px/5px
18. enhance sharpen 90
19. set fg to green
20. bucket fill opacity 15%, fill whole selection
21. unselect
22. add new layer grid
23. filter render grid

I’ll flesh this out more, but I wanted to get it up so others could use it.

I’d like to merge this into [donjon’s dungeon generator](http://donjon.bin.sh/d20/dungeon/index.cgi) source by using the gimp API; if I could do that, it’d be pretty badass if I could automate the whole process.