# Animation
___

### Frames
we have [5 images](https://drive.google.com/file/d/1bMVMUv-B0XF9nBJfw_7ek781cmUjrdgn/view)
<span>
<img src="https://sheepolution.com/images/book/17/jump1.png">
<img src="https://sheepolution.com/images/book/17/jump2.png">
<img src="https://sheepolution.com/images/book/17/jump3.png">
<img src="https://sheepolution.com/images/book/17/jump4.png">
<img src="https://sheepolution.com/images/book/17/jump5.png">
</span>

to make it an animation in the game
first we need to load the images, and put them in a table

```lua
function love.load()
	frames = {}
	
	for i=1,5 do
		table.insert(frames, love.graphics.newImage("jump" .. i .. ".png"))
	end
end
```

okay now we loaded the images, how we create an image?
for-loop? but no, it'd draw all the frames at the same time.

we want to draw a different frame every second
* so we need a variable that increases by 1 every second

```lua
function love.load()
	frames = {}
	
	for i=1,5 do
		table.insert(frames, love.graphics.newImage("jump" .. i .. ".png"))
	end
	currentFrame = 1
end

function love.update(dt)
	currentFrame = currentFrame + dt
end

function love.draw()
	love.graphics.draw(frames[currentFrame])
end
```

yeah, close. if we run the game we'll get an error:
`bad argument #1 to 'draw'(Drawable expected, got nil)`
* because `dt` has decimals and that makes `currentFrame` has decimals
* and we certainly can't do something like `frames[1.016]`

to solve it we need to round the number down with `math.floor()`
* so 1.016 will become 1
```lua
function love.draw()
	love.graphics.draw(frames[math.floor(currentFrame)])
end
```

yeah it works, until `currentFrame` gets to 6
* since we only have `jump1.png`, `jump2.png`, ..., `jump5.png`, but no `jump6.png`

```lua
function love.update(dt)
	currentFrame = currentFrame + 10 * dt -- let's speed up the animation
	if currentFrame >= 6 then
		currentFrame = 1
	end
end
```

![jump.gif (204×260) (sheepolution.com)](https://sheepolution.com/images/book/17/jump.gif)
___

### Quads
okay it works, but it's not very efficient.

* with large animations we're going to need a lot of images
* usually we wouldn't hand to the program a lot of images, but only 1 big image
	* then draw part of the image each frame

![jump.png (585×233) (sheepolution.com)](https://sheepolution.com/images/book/17/jump.png)

so this time we'll load only 1 image
```lua
function love.load()
	image = love.graphics.newImage("jump.png")
end
```

Imagine quads like a rectangle that we cut out of the image, like cutting a piece of paper
* then we tell the game "we want this part of the image for this frame"
* this is what the function [`love.graphics.newQuad`](https://love2d.org/wiki/love.graphics.newQuad) does
	* first arguments are the x and y position of the quad
	* next 2 arguments are the width and height of the quad
		* in our example, they're 117 and 233, respecitvely
	* last 2 arguments are the width and height of the full image

```lua
function love.load()
	image = love.graphics.newImage("jump.png")
	frames = {}
	local frame_width = 117
	local frame_height = 233
	table.insert(frames, love.graphics.newQuad(0, 0, frame_width, frame_height, image:getWidth(), image:getHeight()))
	
	currentFrame = 1
end
```

now let's try drawing it
```lua
function love.draw()
	love.graphics.draw(image, frames[1], 100, 100)
end
```
![quad_position.png (218×309) (sheepolution.com)](https://sheepolution.com/images/book/17/quad_position.png)

* remember quad doesn't care about where we gonna draw it
	* it's a matter to `love.graphics.draw()`

Then, to draw the next frame we need to move the rectangle to the right
* in our example, we need to move the rectangle's x to the right by 117
* and same goes for the 3rd quad, and so forth
![jump_help.png (633×279) (sheepolution.com)](https://sheepolution.com/images/book/17/jump_help.png)

It seems we're repeating the action, so let's use a for-loop
```lua
function love.load()
	image = love.graphics.newImage("jump.png")
	
	-- prevent calling the functions multiple times
	local width = image:getWidth()
	local height = image:getHeight()
	
	frames = {}
	
	local frame_width = 117
	local frame_height = 233
	
	-- notice we start on 0 and end on 4, instead of 1 to 5
	for i=0,4 do
		table.insert(frames, love.graphics.newQuad(i * frame_width, 0, frame_width, frame_height, width, heigh))
	end
	
	currentFrame = 1
end
```

finally change back the `draw()` function to work with `currentFrame` again
```lua
function love.draw()
	love.graphics.draw(image, frames[math.floor(currentFrame)], 100, 100)
end
```
___

### Multiple rows

the image in the above example is in one row
but how about this?
![jump_2.png (351×466) (sheepolution.com)](https://sheepolution.com/images/book/17/jump_2.png)
it's not that hard actually,
we just need to loop through one more row
* but, since there may be a third, a forth, a fifth, ... rows
* it's better to use a for-loop
	* so it's gonna be for-loop inside a for-loop

```lua
function love.load()
	image = love.graphics.newImage("jump_2.png")
	local width = image:getWidth()
	local height = image:getHeight()
	
	frames = {}
	
	local frame_width = 117
	local frame_height = 233
	
	for i=0,1 do
		for j=0,2 do
			table.insert(frames, love.graphics.newQuad(j * frame_width, i * frame_height, frame_width, frame_height, width, height))
		end
	end
	
	currentFrame = 1
end
```

notice this way we have an extra, empty quad
* which isn't really a big deal since we'll only draw the first 5 frames in the `love.draw()` function
* still, it's not ideal
	* so it's better to prevent it

```lua
function love.load()
	...
	maxFrames = 5
	for i=0,1 do
		for j=0,2 do
			table.insert(frames, love.graphics.newQuad(j * frame_width, i * frame_height, frame_width, frame_height, width, height))
			if #frames == maxFrames then
				-- so this is the 6th quad
				break -- end the for-loop
			end
		end
		print("I don't break!") -- notice this still gets printed
		-- if we don't want the outer loop continues we need the same if-statement here
	end
end
```
___

### Bleeding

> [!definition]
> When rotating and/or scaling an image while using quads, part of the image outside of the quad could get drawn. This effect is called *bleeding*.

eg.
<div style="display: flex; align-items: center;">
<img src="https://sheepolution.com/images/book/17/rectangles1.png">
<span style="font-size: 2rem;">&rarr;</span>
<img src="https://sheepolution.com/images/book/17/bleeding.png">
</div>
* Why it happens is kinda technical, the fact is that it does happen...

We can solve this issue by adding a 1 pixel border around the frame
* can either be the same color as the actual border
* or just let it be transparent
![bleeding_fix.png (240×141) (sheepolution.com)](https://sheepolution.com/images/book/17/bleeding_fix.png)

and then we don't include that border inside the quad
* in our example we placed a purple frame there as the border
![jump_3.png (357×470) (sheepolution.com)|300](https://sheepolution.com/images/book/17/jump_3.png)

okay, now what should we put in the `love.graphics.newQuad()` function?
```lua
love.graphics.newQuad(1 + j * frame_width, 1 + i * frame_height, frame_width, frame_height, width, height)
```

* this only work for the first frame
* the frames after are gonna look like this
<img src="https://sheepolution.com/images/book/17/almost.png" style="background-color: white;">

that's because we forgot to skip 2 pixels each frame
```lua
love.graphics.newQuad(1 + j * (frame_width + 2), 1 + i * (frame_height + 2), frame_width, frame_height, width, height)
```
<img src="https://sheepolution.com/images/book/17/whatisgoingon.png" style="background-color: white;">
___

# Tiles
in a lot of 2D games the levels are made out of tiles

1. let's start off by creating a single line
* create a table and fill it with ones and zeros
```lua
function love.load()
	tilemap = {1, 0, 0, 1, 1, 0, 1, 1, 1, 0}
end
```

this is our level
* `1` is a tile
* `0` is empty

2. now we draw the level
```lua
function love.draw()
	for i,v in ipairs(tilemap) do
		if v == 1 then
			love.graphics.rectangle("line", i * 25, 100, 25, 25)
		end
	end
end
```
![one_row.png (276×85) (sheepolution.com)](https://sheepolution.com/images/book/18/one_row.png)

3. so it works. then let's add more lines to our level.
* we do this by putting tables inside a table, also known as a 2D table
```lua
function love.load()
	tilemap = {
		{1, 1, 1, 1, 1, 1, 1, 1, 1, 1},
		{1, 0, 0, 0, 0, 0, 0, 0, 0, 1},
		{1, 0, 0, 1, 1, 1, 1, 0, 0, 1},
		{1, 0, 0, 0, 0, 0, 0, 0, 0, 1},
		{1, 1, 1, 1, 1, 1, 1, 1, 1, 1}
	}
end
```

* with 2D tables we access the values like `tilemap[4][3]`
	* it means the 3rd value of the 4th table
	* or the 3rd column on the 4th row
![2d_table.gif (463×249) (sheepolution.com)](https://sheepolution.com/images/book/18/2d_table.gif)

4. let's draw the level again
* this time since it's a 2D table, we need to use a for-loop inside a for-loop
	* also known as a *nested for-loop*
```lua
function love.draw()
	for i,row in ipairs(tilemap) do
		for j,tile in ipairs(row) do
			if tile == 1 then
				love.graphics.rectangle("line", j * 25, i * 25, 25, 25)
			end
		end
	end
end
```

5. surely we don't necessarily need to put ones and zeros as the values
* maybe we can use different numbers for different colors
```lua
function love.load()
	tilemap = {
		{1, 1, 1, 1, 1, 1, 1, 1, 1, 1},
		{1, 2, 2, 2, 2, 2, 2, 2, 2, 1},
		{1, 2, 3, 4, 5, 5, 4, 3, 2, 1},
		{1, 2, 2, 2, 2, 2, 2, 2, 2, 1},
		{1, 1, 1, 1, 1, 1, 1, 1, 1, 1}
	}
	
	-- fill the colors table with RGB numbers
	colors = {
		{1, 1, 1},
		{1, 0, 0},
		{1, 0, 1},
		{0, 0, 1},
		{0, 1, 1},
	}
end

function love.draw()
	for i,row in ipairs(tilemap) do
		for j,tile in ipairs(row) do
			if tile ~= 0 then
				love.graphics.setColor(colors[tile])
				love.graphics.rectangle("fill", j * 25, i * 25, 25, 25)
			end
		end
	end
end
```
![colors.png (278×158) (sheepolution.com)](https://sheepolution.com/images/book/18/colors.png)
___

### Images in tiles

1. we want to use this image as tile instead of just color  <span><img src="https://sheepolution.com/images/book/18/tile.png"></span>

```lua
function love.load()
	image = love.graphics.newImage("tile.png")
	
	width = image:getWidth()
	height = image:getHeight()
	
	tilemap = {
		{1, 1, 1, 1, 1, 1, 1, 1, 1, 1},
		{1, 2, 2, 2, 2, 2, 2, 2, 2, 1},
		{1, 2, 3, 4, 5, 5, 4, 3, 2, 1},
		{1, 2, 2, 2, 2, 2, 2, 2, 2, 1},
		{1, 1, 1, 1, 1, 1, 1, 1, 1, 1}
	}
	
	colors = {
		{1, 1, 1},
		{1, 0, 0},
		{1, 0, 1},
		{0, 0, 1},
		{0, 1, 1},
	}
end

function love.draw()
	for i,row in ipairs(tilemap) do
		for j,tile in ipairs(row) do
			if tile ~= 0 then
				love.graphics.setColor(colors[tile])
				love.graphics.draw(image, j * width, i * height)
			end
		end
	end
end
```
![colors_image.png (376×216) (sheepolution.com)](https://sheepolution.com/images/book/18/colors_image.png)

2. but surely drawing different images would be a more common use case
* previously we talked about how we can use quads for animations
* quads can be used for tiles as well
<img src="https://sheepolution.com/images/book/18/tileset.png" style="background-color: white;">

* creating quads
```lua
function love.load()
	image = love.graphics.newImage("tileset.png")
	
	local image_width = image:getWidth()
	local image_height = image:getHeight()
	
	-- 1. we know the size of each tile
	width = 32
	height = 32
	
	-- 2. we don't know the size of the tiles,
	--    so we count the rows and columns in the tileset
	width = (image_width / 3) - 2   -- subtract 2 for the pixels for prevent bleeding
	height = (image_height / 2) - 2
	
	-- create the quads
	quad = {}
	
	for i=0,1 do
		for j=0,2 do
			table.insert(
				quads,
				love.graphics.newQuad(
					1 + j * (width + 2),
					1 + i * (height + 2),
					width, height,
					image_width, image_height
				)
			)
		end
	end
	
	tilemap = {
		{1, 1, 1, 1, 1, 1, 1, 1, 1, 1},
		{1, 2, 2, 2, 2, 2, 2, 2, 2, 1},
		{1, 2, 3, 4, 5, 5, 4, 3, 2, 1},
		{1, 2, 2, 2, 2, 2, 2, 2, 2, 1},
		{1, 1, 1, 1, 1, 1, 1, 1, 1, 1}
	}
end
```

* this is how the `quad` table looks like
<img src="https://sheepolution.com/images/book/18/numbered.png" style="background-color: white;">

last thing, let's draw this
![level.png (128×128) (sheepolution.com)](https://sheepolution.com/images/book/18/level.png)

```lua
function love.load()
	...
	tilemap = {
		{1, 6, 6, 2, 1, 6, 6, 2},
		{3, 0, 0, 4, 5, 0, 0, 3},
		{3, 0, 0, 0, 0, 0, 0, 3},
		{4, 2, 0, 0, 0, 0, 1, 5},
		{1, 5, 0, 0, 0, 0, 4, 2},
		{3, 0, 0, 0, 0, 0, 0, 3},
		{3, 0, 0, 1, 2, 0, 0, 3},
		{4, 6, 6, 5, 4, 6, 6, 5}
	}
end

function love.draw()
	for i,row in ipairs(tilemap) do
		for j,tile in ipairs(row) do
			if tile ~= 0 then
				love.graphics.draw(image, quads[tile], j * width, i * height)
			end
		end
	end
end
```
___

### Player in tiles
next, let's create a player that can walk around
* but not go through walls
* ![player.png (32×32) (sheepolution.com)](https://sheepolution.com/images/book/18/player.png)

```lua
function love.load()
	...
	player = {
		image = love.graphics.newImage("player.png"),
		tile_x = 2,
		tile_y = 2
	}
end
```

* `tile_x` and `tile_y` is the player's position on our tilemap
	* multiply the number by the tile width and height when drawn

but first, let's make it moves
* instead of giving him a smooth movement, we just make him jump to its next position
* therefore we won't need `dt` this time
* also we don't want to know if the movement keys are down, but if they are pressed
	* we don't need `love.keyboard.isDown()`, but we need `love.keypressed()`

```lua
function love.keypressed(key)
	local x = player.tile_x
	local y = player.tile_y
	
	if key == "left" then
		x = x - 1
	elseif key == "right" then
		x = x + 1
	elseif key == "up" then
		y = y - 1
	elseif key == "down" then
		y = y + 1
	end
	
	player.tile_x = x
	player.tile_y = y
end
```

finally, let's draw it
```lua
function love.draw()
	for i,row in ipairs(tilemap) do
		for j,tile in ipairs(row) do
			if tile ~= 0 then
				love.graphics.draw(image, quads[tile], j * width, i * height)
			end
		end
	end
	-- draw the player
	love.graphics.draw(player.image, plyaer.tile_x * width, player.tile_y * height)
end
```

now the player can walk around
* although he can walk through walls
![tile-move-1.gif (313×287) (sheepolution.com)](https://sheepolution.com/images/book/18/tile-move-1.gif)

next, we need to check if the position the player goes is a wall there
* we first make a function to check if a position is a wall or not
```lua
function isEmpty(x, y)
	-- y is the row, x is the column. i know it looks weird.
	return tilemap[y][x] == 0
end
```

then now we execute the check when ever the player try to move
```lua
function love.keypressed(key)
	...
	if isEmpty(x, y) then
		player.tile_x = x
		player.tile_y = y
	end
end
```
![tile-move-2.gif (290×287) (sheepolution.com)](https://sheepolution.com/images/book/18/tile-move-2.gif)

There we go. Not so hard, eh?
Try to add more features to it if you like.
* eg. pick things up, opens a door when the player touch a key, etc.
___

# Audio

here are some audio files for example
* sound effect: https://sheepolution.com/images/book/19/sfx.ogg
* music: https://sheepolution.com/images/book/19/song.ogg

1. first we need to create the audio
* it's called a *source*, the source of the audio
* using a function `love.audio.newSource(path)`, which takes 2 arguments
	* first is the path to the file
	* second is what type of source is
		* either `"static"` or `"stream"`
		* LÖVE wiki: "A good rule of thumb is to use stream for music files and static for all short sound effects"
			* because we want to avoid loading large files at once
			* that is to say, don't load a minute-long music using `"static"`

* we'll start with a song and make it loops endlessly
	* so we use `"stream"`
```lua
function love.load()
	song = love.audio.newSource("path/to/song.ogg", "stream")
end
```

2. next, play the song
* there are 2 ways to do that
```lua
function love.load()
	song = love.audio.newSource("path/to/song.ogg", "stream")
	
	-- 1.
	love.aduio.play(song)
	-- 2.
	song:play()
end
```
* there isn't really a different between the two
	* so use which ever you like

but it'll only play once. it doesn't loop.
* try using the [docs](https://love2d.org/wiki/Source)
* we need `Source:setLooping()`
```lua
function love.load()
	song = love.audio.newSource("path/to/song.ogg", "stream")
	song:setLooping(true)
	song:play()
end
```

There we go. we have looping background music now.
Adding a sound effect isn't really any different
* let's make it play every time we press Space
```lua
function love.load()
	...
	-- sfx is short for 'sound effect'
	sfx = love.audio.newSource("sfx.ogg", "static")
end

function love.keypressed(key)
	if key == "space" then
		sfx:play()
	end
end
```

next up, read the docs for
* [set the volume](https://love2d.org/wiki/Source:setVolume)
* [pause](https://love2d.org/wiki/Source:pause) a source
* get the source's [position](https://love2d.org/wiki/Source:tell) in time
___
