---
author: Jesse Morgan
categories:
- Uncategorized
date: "2014-11-07T15:13:33Z"
guid: http://morgajel.net/?p=1574
id: 1574
title: The Key to Creating Good, Tileable Images (in GIMP)
url: /2014/11/07/1574
---

I’m writing this as a general guide both for future reference, and to get feedback from others.

Often when using an image manipulation program such as GIMP or Photoshop, you’ll need to create large swaths of consistent texture. The easiest way to do this is with a pattern fill tool, however most programs only include a small set of patterns. The good news is that you can make your own with relatively little grief.

A quick note- While you may occasionally want an obvious tile (e.g. tiled floors), this discussion will focus on tiles that try to appear seamless.

## Choose a Texture

<div class="wp-caption alignright" id="attachment_1593" style="width: 310px">[![dirt](http://morgajel.net/wp-content/uploads/2014/11/dirt-300x168.jpg)](http://morgajel.net/wp-content/uploads/2014/11/dirt.jpg)For our example, I took a photo of the dirt outside. As you can see, it’s not perfectly uniform, but we’ll take care of that in a moment.

</div>The first step is obviously to decide what you’d like to have as a texture- dirt, cement, gravel, treebark, marble, and leaves are all good examples of common textures. **Your texture should be relatively consistent.** While some variation is needed to give it flavor, it needs to be somewhat symmetric (i.e. a baseball in a tile of grass will make tiling obvious), however you may be able to cover that up.

## Crop Inconsistencies

<div class="wp-caption alignright" id="attachment_1594" style="width: 310px">[![dirt_cropped](http://morgajel.net/wp-content/uploads/2014/11/dirt_cropped-300x214.jpg)](http://morgajel.net/wp-content/uploads/2014/11/dirt_cropped.jpg)Here you can see I cropped out the large flat top right corner and dark bottom left corner. It’s starting to look a lot more consistent, however there’s still a few pesky details that stick out.

</div>Sometimes your source picture will have an anomaly on one side, such as the edge of a sidewalk on a dirt texture, or stick in a field of grass that wasn’t quite out of frame. The simplest way to deal with this is to crop it out. Save as much as you can of the original image, but make sure to completely remove the inconsistency. Judicious cropping can also help you determine your focus- is your pattern a field of grass or blades of grass?

## Remove Anomalies

<div class="wp-caption alignright" id="attachment_1596" style="width: 310px">[![dirt_anomolies](http://morgajel.net/wp-content/uploads/2014/11/dirt_anomolies-300x214.jpg)](http://morgajel.net/wp-content/uploads/2014/11/dirt_anomolies.jpg)Here you can see I removed quite a few wood chips, the twig, some tree buds, and a small sprout The consistency is coming along nicely at this point.

</div>Should your texture has some type of anomaly (like the baseball mentioned above) that is too far in to safely crop, you can often use a combination of the rubber stamp tool and healing tool to copy a more generic spot over the anomaly and blend it into place.

## Offset and Wrap

<div class="wp-caption alignright" id="attachment_1595" style="width: 310px">[![dirt_offset](http://morgajel.net/wp-content/uploads/2014/11/dirt_offset-300x214.jpg)](http://morgajel.net/wp-content/uploads/2014/11/dirt_offset.jpg)We were relatively lucky with the dirt; the seams are barely noticeable.

</div>Now that our tile looks fairly consistent, lets examine the seams. For this we’ll need to offset the entire image by half (Layer-&gt;transform-&gt;offset or ctrl+shift+o) along the X and Y axis. This should give you a nice cross where the edges will meet in the final product. Quite often you will find color variation between the two sides- sometimes you can get lucky and dodge or burn the image to get them closer shades, sometimes it’s far more tricky (and beyond the scope of this document).

Using the four quadrants as a baseline, you can use the rubber stamp and healing tools to cover the seams- with any luck this process will be fairly simple and painless. Remember, the goal is to make the seams disappear, so be sure to feather it in unevenly, and not with a straight line that will still be visible.

<div class="wp-caption alignright" id="attachment_1597" style="width: 310px">[![dirt_offset_final](http://morgajel.net/wp-content/uploads/2014/11/dirt_offset_final-300x214.jpg)](http://morgajel.net/wp-content/uploads/2014/11/dirt_offset_final.jpg)This image should be seamlessly tileable at this point.

</div>Once complete, we need to verify we didn’t accidentally damage leave any artifacts need the ends of the seams. To do this, do another offer, but only offset the Y by half; this may reveal a small horizontal seam near the center. Take care of that and perform a final offset, transition X by half. This should leave a small vertical seam. Once it’s resolved, you should have a nice, seamless texture… but we’re not done yet.

<div class="wp-caption alignright" id="attachment_1598" style="width: 310px">[![dirt_quad](http://morgajel.net/wp-content/uploads/2014/11/dirt_quad-300x214.jpg)](http://morgajel.net/wp-content/uploads/2014/11/dirt_quad.jpg)Unfortunately we do see some banding along the top and middle of the image; this could be corrected with some dodging and burning, but we’ll call it good for now.

</div>## Quadruple It

The phrase “can’t see the forest for the trees” applies here. We’ve seen what one intersection looks like- how about several? If we increase the image canvas size and duplicate time layer 3 more times, we can set them side-by-side and merge them down to identify redundant features that escaped us previously. Things you might see include:

- That small twig may have been unnoticeable with one tile, but with many tiles it betrays the redundancy
- There may have been an ever-so slight variation in color that was previously unnoticed
- The area surrounding the original seams was not as well blended as previously thought
- A small area that is simply too unique and sticks out just enough to be noticeable.

You have a choice at this point; you can either undo back to the single image, or choose to keep it quadrupled. If you keep it quadrupled, you can ad ever-so-slight modifications to each quadrant to help disperse the redundancy.

## Exporting

The final step is to export (File-&gt;export as… or ctrl+shift+e) and save the pattern as a .pat file. This should be kept in your GIMP patterns folder (on linux ~/.gimp-2.8/patterns/)

The next time you refresh your patterns box, you should see your new texture.

## Holy Cow it’s too Big!

Oh, all that work we just put in? It may be worthless; I shoulda mentioned that up front.

Here’s the problem: If your base texture is 2500×2000 pixels, don’t be surprised that your pattern is gigantic when you try to use it. As of right now, GIMP doesn’t have a built-in way to scale patterns (although there are plugins that claim to do it). Your best bet is to scale the image down before exporting it to a pat file, just be warned that the scaled image may have seams reappear from the scaling interpolation, so you may need to run through the offsets again to verify that it’s acceptable.

## Final Note…

- No, I cannot tell you how to do this in photoshop.
- I despise capitalizing GIMP. it I understand why it’s supposed to be, but it still feels super lame.
- If you have feedback, please leave a comment below- I’d love to improve my process.
- If you liked this tutorial, please consider supporting me via [Patreon](http://www.patreon.com/morgajel)