# 32 Color Palette

![db32_v1_analyze.png (537×385) (bahnhof.se)](http://privat.bahnhof.se/wb364826/pic/db32_v1_analyze.png)
this is a very famous 32 color palette called *DawnBringer's 32 Color Palette*

![[32-colors-grid.png]]
this picture is done with just the 32 colors
* the colors are *dithered*
	* meaning just draw two color pixels by pixel interleaved
	* result is that when looking from a far distance, it looks like a brand new color
* it's a dithering chart

the left-most column and the top-most row, are the 32 colors
* all other cells are the outcome of the colors intersected with each other
* inside each cell, it's just pixels dithered with the 2 colors

![db16_v1_mockup.png (508×350) (bahnhof.se)](http://privat.bahnhof.se/wb364826/pic/db16_v1_mockup.png)
these are actually done with 16 colors

even for an real image that has more than a thousand colors
* using 32 color palette can still look quite similar
* except a regular image would have a lot of blur, a lot of distorted color
	* the 32 color palette can't mimic most of them
	* resulting some blotchy patterns

if we're going for something less intensive
* eg. golden age comic book cover
* thousands of colors, for shades and stuff like that, can still work well with just 32 colors
___

# Why it's good?

first of all, the art for our Match 3 game actually using a 32 color palette
* Breakout used the same palette
* our future 2D lectures as well

if we're trying to draw sprite art, 8 or 16 or 32 color palette is a quick and easy way to give our arts a little bit of consistency
* our work will look much more cohesive

it's used to be a real world constraint of the hardware
* NES only had 4 colors or so, for each sprite
* if we're going for a retro look, gotta stick with the limited number of colors

we can do *palette swap*
* if we want to draw Mario with different colors in game
	* the color in the sprite is map out the some tables, like 1 equals red, 2 equals blue, ...
	* then we can just shift the map to map to another color palette, to get maybe a blue Mario
* this is actually how Super Mario Bros used to do its clouds and bushes
	* clouds and bushes are actually the same sprite
	* just different colors
___
