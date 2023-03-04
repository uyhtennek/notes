# bird-0

`love.graphics.newImage(path)`
* loads an image from a graphics file
	* JPEG, PNG, GIF, etc.
* stores it in an object

```lua
push = require 'push' -- use virtual resolution

WINDOW_WIDTH = 1280
WINDOW_HEIGHT = 720
VIRTUAL_WIDTH = 512
VIRTUAL_HEIGHT = 288

local background = love.graphics.newImage('background.png')
local ground = love.graphics.newImage('ground.png') -- foreground

function love.load()
	love.graphics.setDefaultFilter('nearest', 'nearest') -- no blurriness
	
	love.window.setTitle('Fifty Bird')
	
	push:setupScreen(VIRTUAL_WIDTH, VIRTUAL_HEIGHT, WINDOW_WIDTH, WINDOW_HEIGHT, {
		vsync = true,
		fullscreen = false,
		resizable = true
	})
end

function love.resize(w, h)
	push:resize(w, h)
end

function love.keypressed(key)
	if key == 'escape' then
		love.event.quit()
	end
end

function love.draw()
	push:start()   -- same as push:apply('start')
	love.graphics.draw(background, 0, 0)
	love.graphics.draw(ground, 0, VIRTUAL_HEIGHT - 16)
	push:finish()  -- same as push:apply('finish')
end
```
___

# Parallax Scrolling

> [!definition]
> It refers to the illusion of movement, given two frames of reference that are moving at different rates (or speed if it makes more sense).

eg. You're diving on the highway
* a fence that next to you moves very fast
* mountains that are very far move very slow

## bird-1
```lua
...
local background = love.graphics.newImage('background.png')
local backgroundScroll = 0

local ground = love.graphics.newImage('ground.png')
local groundScroll = 0

local BACKGROUND_SCROLL_SPEED = 30
local GROUND_SCROLL_SPEED = 60

-- need either 2 copies of the same image with the screen width
-- or 1 big image with double screen width (here we used this approach)
local BACKGROUND_LOOPING_POINT = 413
...
function love.update(dt)
	backgroundScroll = (backgroundScroll + BACKGROUND_SCROLL_SPEED * dt)
		% BACKGROUND_LOOPING_POINT
	
	groundScroll = (groundScroll + GROUND_SCROLL_SPEED * dt)
		% VIRTUAL_WIDTH
end

function love.draw()
	push:start()
	love.graphics.draw(background, -backgroundScroll, 0)
	love.graphics.draw(ground, -groundScroll, 0)
	push:finish()
end
```

* notice the width of the image is 1157 px
	* our virtual screen width is 512 px, meaning we should just need a width of 1024 px
* actually it doesn't matter as long as
	* the image is longer than the screen width
	* the ending part of the image looks the same as the starting part of the image
		* or at least 2 parts of the image looks exactly the same

* the end result is that the image can loop smoothly
	* instead of suddenly jump to a weird position
___

## Games are illusion

eg. The Lengend of Zelta: Ocarina of Time
https://www.youtu.be/HUGE9L7V4oY

1. shop keeper doesn't have legs
2. mountains are cut in half

only the parts that the player can see, get loaded and rendered
* similar to how people create stages in real life, to make us feel like we're actually in a scene

> [!quote]
> If you're trying to achieve a particularly grand effect. Something to think about, is how can I make it seem like I'm doing something, but I'm actually not.

It seems like the bird is flying through an infinite series of levels
* but "infinite" actually is limited
* it's a game after all, a program running on our hardware and limited by our hardware
___

# bird-2: make a bird

```lua
--file: Bird.lua
Bird = Class{} -- using hump.class

function Bird:init()
	self.image = love.graphics.newImage('bird.png')
	-- Image is a class that löve gave us, so there are some functions löve implemented
	self.width = self.image:getWidth()
	self.height = self.image:getHeight()
	
	self.x = VIRTUAL_WIDTH / 2 - (self.width / 2)
	self.y = VIRTUAL_HEIGHT / 2 - (self.height / 2)
end

function Bird:render()
	love.graphics.draw(self.image, self.x, self.y)
end
```

```lua
--file: main.lua
...
Class = require 'class'
require 'Bird'
...
local bird = Bird()
...
function love.draw()
	...
	bird:render()
	push:finish()
end
```
___

# bird-3: gravity

we want to increment gravity, over and over again, frame by frame, some constant value. then add that to an object's or objects' y value
* so that things fall faster, and faster
* like in real life

```lua
--! file: main.lua
...
function love.update(dt)
	...
	bird:update(dt) -- defer the funtionality to the Bird class
end
```

```lua
--! file: Bird.lua
Bird = Class{}

local GRAVITY = 20 -- just test and tune

function Bird:init()
	...
	self.dy = 0
end

function Bird:update(dt)
	self.dy = self.dy + GRAVITY * dt
	self.y = self.y + self.dy
end
...
```
___

## bird-4: anti-gravity

![exploring-game-space-teaser.png (1013×423) (nyu.edu)](https://game.engineering.nyu.edu/wp-content/uploads/2013/08/exploring-game-space-teaser.png)

how do we implement jumping?
1. we already have gravity, which increments frame by frame over time
2. if we want the bird to jump, its gravity should be some negative value
	* so instead of falling down, it jumps up
	* so when ever it jumps, gravity should be -5
3. next, since gravity is still increasing, the value will get higher
	* its gravity next frame is: $-5\times20\times dt$
		* 20 because we defined the gravity increase by 20 every second

the result is that
the bird shoot up pretty fast, but immediately after gravity is gonna start taking hold

```lua
--! file: main.lua
...
function love.load()
	...
	love.keyboard.keysPressed = {}
end
```

the bird should jump when we press 'space'
* we can pass the `key` argument from `love.keypressed(key)` in `main.lua` to our `Bird` class
	* but what if we have a second `Bird` instance, or 20 or 30 classes that would perform something when ever we press 'space'
* therefore, in these cases, we should delegate the 

`love.keyboard.keysPressed`
* `love.keyboard` is a table from Lua, but `love.keyboard.keysPressed` is a thing we created
	* it's not part of `love` or `love.keyboard`
* and in Lua, we can manipulate tables however we want
	* here we set a key `keyPressed` to it and assign an empty table as its value

```lua
--! file: main.lua
function love.keypressed(key)
	love.keyboard.keysPressed[key] = true
	...
end

function love.keyboard.wasPressed(key)
	return love.keyboard.keysPressed[key]
end
...
function love.update(dt)
	...
	love.keyboard.keysPressed = {}
end
```

it's better to keep all the `love` functions or our main functions in `main.lua`
* if we declare anothe function in another file with the same name, the original function in `main.lua` could get overwritten
	* depends on what order does the files get loaded in
* so that we don't have to worry about what functions are actually valid

`love.keyboard.wasPressed(key)`
* this is actually not a `love` function, but we created it and assign it to `love.keyboard`

so what we're doing here?
what the `keyPressed` is gonna look like when ever we pressed a key?
* it's simple actually.
1. when ever we pressed a key, we add that key to `keyPressed`
2. at the end of each frame, we set `keyPressed` back to an empty table

ultimately, why we're doing this, is that
in other files, we can query `keyPressed` to see if a key or some keys are pressed
* we're not passing the `key` to other files from `main.lua`
* instead, in what ever file that want to know if a key is pressed, they query `love.keyboard.wasPressed` in `main.lua`

```lua
--! file: Bird.lua
function Bird:update(dt)
	self.dy = self.dy + GRAVITY * dt
	if love.keyboard.wasPressed('space') then
		self.dy = -5
	end
	self.y = self.y + self.dy
end
```
___
