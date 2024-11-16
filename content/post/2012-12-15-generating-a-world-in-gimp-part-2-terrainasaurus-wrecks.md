---
author: Jesse Morgan
categories:
- Uncategorized
date: "2012-12-15T00:05:06Z"
guid: http://morgajel.net/?p=1249
id: 1249
title: 'Generating a world in Gimp, Part 2: Terrainasaurus wrecks'
url: /2012/12/15/1249
---

Ok, so lets come back to the landmasses

1. Click on the land layer and color select the land
2. Use Filter-&gt;render-&gt;cloud-&gt;solid noise with the following settings: 
    1. random seed 1056316098
    2. detail 15
    3. turbulent
    4. x,y= 4.0,4.0
3. Select colors-&gt;levels and click auto, then OK

[![world_tut_10](http://morgajel.net/wp-content/uploads/2012/12/world_tut_10.png)](http://morgajel.net/2012/12/15/1249/world_tut_10)

If you want nice, boring land, you can color this with the technique below and be done. Otherwise we should spice it up a bit.

With your landmasses selected

1. Create a new layer
2. Click on the land layer and color select the land
3. Use Filter-&gt;render-&gt;cloud-&gt;solid noise with the following settings: 
    1. random seed **2925538520** (note that this is different)
    2. detail 15
    3. turbulent
    4. x,y= 6.0,6.0 (note that this is different)
4. Select colors-&gt;levels and click auto, then OK
5. Set this new layer’s mode to hard light
6. Merge down

**[![world_tut_12](http://morgajel.net/wp-content/uploads/2012/12/world_tut_121.png)](http://morgajel.net/2012/12/15/1249/world_tut_12-2)**

Perfect! Now your land still roughly matches the patterns of the original solid noise, but has a flavor of something new as well. Now lets colorize this badboy Here’s a simple gradient that I made called Terrain.ggr. It can be dropped into your gimp-2.8/gradient directory.

```
GIMP Gradient
Name: Terrain
5
0.000000 0.042056 0.144860 0.909804 0.906572 0.635079 1.000000 0.084619 0.588235 0.032295 1.000000 0 0 0 0
0.144860 0.285047 0.485981 0.084619 0.588235 0.032295 1.000000 0.024864 0.133333 0.013595 1.000000 0 0 0 0
0.485981 0.593458 0.686916 0.024864 0.133333 0.013595 1.000000 0.254902 0.199829 0.031988 1.000000 0 0 0 0
0.686916 0.761682 0.845794 0.254902 0.199829 0.031988 1.000000 0.164706 0.070252 0.018731 1.000000 0 0 0 0
0.845794 0.922897 1.000000 0.164706 0.070252 0.018731 1.000000 1.000000 1.000000 1.000000 1.000000 1 0 0 0
```

[![world_tut_8](http://morgajel.net/wp-content/uploads/2012/12/world_tut_8.png)](http://morgajel.net/2012/12/15/1249/world_tut_8)

Once you get that loaded in and gimp restarted, we can continue.

1. Use the gradient tool and select your new terrain gradient
2. Color select the empty space around your landmasses
3. Select Select-&gt;Invert
4. Select Colors-&gt;Map-&gt;gradient map
5. Select Select-&gt;None
6. Select Filters-&gt;Blur-&gt;Blur

And now your map has terrain!

[![world_tut_13](http://morgajel.net/wp-content/uploads/2012/12/world_tut_13-300x200.png)](http://morgajel.net/wp-content/uploads/2012/12/world_tut_13.png)