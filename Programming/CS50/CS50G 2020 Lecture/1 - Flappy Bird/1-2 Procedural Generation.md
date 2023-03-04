# bird-5: Infinite Pipe

firstly, what happen if we keep spawning new pipes?
* surely at some point the computer runs out of memory to spawn a new pipe
* how much memory is a pipe using?
	* for each pipe, it has a `x, y, width, height`, and a image reference
	* but notice, it's not a new image for each pipe, just a new image reference
		* because all pipes reference the same sprite image

when the computer does run out of memory
* it'll either hang infinitely or crash

```lua
--! file: main.lua
...
require 'Bird'
require 'Pipe'
...
```
remember an important concepts, is that we want to abstract things as much as possible
* try to put a group of functionality of the same type in a seperate file
* in this game we're just making every entity a class

```lua
--! file: Pipe.lua
Pipe = Class{}

local PIPE_IMAGE = love.graphics.newImage('pipe.png')

local PIPE_SCROLL = -60 -- shifting the pipes to the left
-- 60 pixels per second, since we'll be using `PIPE_SCROLL * dt`

function Pipe:init()
	self.x = VIRTUAL_WIDTH
	self.y = math.random(VIRTUAL_HEIGHT / 4, VIRTUAL_HEIGHT - 10)
	-- covered any where starting from the second quarter of the screen to nearly bottom
	--[[remember every time we used `math.random`, we need to set the random seed
		and typically we'd set it in the top level script, so `main.lua` it is
		if you can't recall what was the function, `math.randomseed(os.time())`]]--
	self.width = PIPE_IMAGE:getWidth()
end

function Pipe:update(dt)
	self.x = self.x + PIPE_SCROLL
end

function Pipe:render()
	love.graphics.draw(PIPE_IMAGE, self.x, self.y)
end
```

notice in here, we had a different approach in referencing an image than `Bird.lua`
```lua
--! file: Bird.lua
...
function Bird:init()
	self.image = love.graphics.newImage('bird.png')
	...
end
...
```

what is the difference?
* for `Bird`, every time we do `init()`, or create an `Bird` object, we allocate memory for an image and load `'bird.png'`
	* so if there are 2 birds, we do this 2 times
	* it's okay in this game because we ever just gonna have 1 bird
		* but surely it turns to a bad things quickly when we have more than just 1 bird
* for `Pipe`, we ever just gonna allocate memory and load `'pipe.png'` once
	* doesn't matter how many pipes there are
	* all of them are just referencing the same image, `PIPE_IMAGE`

That's pretty much it for `Pipe.lua`, now we should start using it in `main.lua`
* the goal here is just spawn a new pipe every 2 seconds
```lua
--! file: main.lua
...
local pipes = {}
local spawnTimer = 0
...
function love.update(dt)
	...
	spawnTimer = spawnTimer + dt
	if spawnTimer > 2 then
		table.insert(pipes, Pipe())
		spawnTimer = 0
	end
	
	bird:update(dt)
	
	for k, pipe in pairs(pipes) do
		pipe:update(dt)
		-- destroy the pipe if it moved beyond the left side of the screen
		if pipe.x < -pipe.width then -- notice it's `-pipe.width` but not `0`
			table.remove(pipes, k)
		end
	end
end
...
function love.draw()
	...
	-- the draw order matters
	-- remember to draw the pipe after background, and before foreground
	love.graphics.draw(background, -backgroundScroll, 0)
	for k, pipe in pairs(pipes) do
		pipe:render()
	end
	love.graphics.draw(ground, -groundScroll, VIRTUAL_HEIGHT - )
	...
end
```

`pairs(pipes)`
* this is a function Lua gives us to iterate a table
* it gives both the key and the value
___

# bird-6: Pipe Pair

```lua
--! file: main.lua
...
require 'Bird'
require 'Pipe'
require 'PipePair' -- so a PipePair object controls 2 Pipe objects
...
-- we no longer use `pipes` because we now store them in pairs
local pipePairs = {} -- every unit is a pair of pipes
local spawnTimer = 0

local lastY = -PIPE_HEIGHT + math.random(80) + 20
...
function love.update(dt)
	...
	if spawnTimer > 2 then
		-- clamp the y value, y is the upper edge of the upper pipe
		local y = math.max(-PIPE_HEIGHT + 10,
			math.min(lastY + math.random(-20, 20), VIRTUAL_HEIGHT - 90 - PIPE_HEIGHT))
		lasty = y
		
		table.insert(pipePairs, PipePair(y))
		spawnTimer = 0
	end
	
	bird:update(dt)
	
	for k, pair in pairs(pipePairs) do
		pair:update(dt)
	end
	
	for k, pair in pairs(pipeParis) do
		if pair.remove then
		    -- so we do removal when ever the remove flag is rised
			table.remove(pipeParis, k)
		end
	end
	...
end

function love.draw()
	...
	for k, pair in pairs(pipePairs) do
		pair:render()
	end
	...
end
```

`require 'PipePair'` and `require 'Pipe'`
* remember there is this important concept of **layering of abstractions**
	* it's common to see in Computer Science in general or games where objects that are composite of objects that are composite of objects
* without a clear hierarchy of abstractions, we'll quickly lose our sanity

`lastY`
* we don't want to spawn the pipes at completely random height, because
	* it doesn't look good, and it is quite unfair, quite hard to beat
* therefore we keep track of the y position of the last `PipePair`

`local y = math.max(-PIPE_HEIGHT + 10, math.min(lastY + math.random(-20, 20), VIRTUAL_HEIGHT - 90 - PIPE_HEIGHT))`
1. `y` is the **start of the upper pipe**
2. `-PIPE_HEIGHT + 10`
	* the minimum y position of the pair
	* `-PIPE_HEIGHT` so we actually start drawing the pipe pair beyond the top of the screen
3. `VIRTUAL_HEIGHT - 90 - PIPE_HEIGHT`
	* the maximum y
	* 90 pixels is the length or height of the gap
	* so `y` is at least the height of the gap and a pipe above the bottom side of the screen
4. `lastY + math.random(-20, 20)`
	* `y` is at most 20 pixels above or below the `lastY`

`PipePair(y)`
* what's gonna happen when we init a `PipePair` object is that
1. we'll have a flipped pipe, right below where `y` is, then the gap, then the unflipped bottom pipe
2. the unflipped pipe is `PIPE_HEIGHT` lower than `y` and 90 pixels lower, for the gap

notice we used a second loop for removing `PipePair` object from the `pipePairs` table
* so that it doesn't skip a loop when there is a removal
* because the values are indexed by numerical indices, but not keys
	* when we remove an item from a table, it shifts every other items down

eg. there is this table `{1, 2, 3}`
* as we iterating through it, we removed the first item
* so in the current iteration, we're looking at index 1
	* after removal, the table becomes `{2, 3}`
* then we come to the next iteration, so we're looking at index 2
	* what is the second item in the current table `{2, 3}`? it's `3`.
	* we effectively skipped the actual second item, `2`
* usually it'd create some buggy behaviors
	* in our case, we can see the very left pipe suddenly stop moving for a frame
	* since it's not updating in that frame but others are

```lua
--! file: PipePair.lua
PipePair = Class{}

local GAP_HEIGHT = 90

function PipePair:init(y)
	-- 32 here just make it a bit further to the right of the screen
	self.x = VIRTUAL_WIDTH + 32
	self.y = y
	
	self.pipes = {
		-- now the Pipe class need some parameters for setting it up
		['upper'] = Pipe('top', self.y)
		['lower'] = Pipe('bottom', self.y + PIPE_HEIGHT + GAP_HEIGHT)
		-- notice we changed the y value for the lower pipe
	}
	
	self.remove = false
end

function PipePair:update(dt)
	if self.x > -PIPE_WIDTH then
		self.x = self.x - PIPE_SPEED * dt
		self.pipes['lower'].x = self.x
		self.pipes['upper'].x = self.x
	else
		self.remove = true
	end
end

function PipePair:render()
	for k, pipe in pairs(self.pipes) do
		pipe:render()
	end
end
```

```lua
--file: Pipe.lua
...
PIPE_HEIGHT = 288 -- about the height of the virtual screen
PIPE_WIDTH = 70

function Pipe:init(orientation, y)
	self.x = VIRTUAL_WIDTH
	self.y = y
	
	self.width = PIPE_IMAGE:getWidth()
	self.height = PIPE_HEIGHT
	
	self.orientation = orientation
end

function Pipe:update(dt)
	-- we don't quite need logic in Pipe Class now
end

function Pipe:render()
	love.graphics.draw(PIPE_IMAGE, self.x,
		(self.orientation == 'top' and self.y + PIPE_HEIGHT or self.y),
		0, 1, self.orientation == 'top' and -1 or 1)
end
```

it turns out `love.graphics.draw()` has a few more parameters
`love.graphics.draw(image, x, y, rotation, x scale, y scale)`

* when we flip an image, the flipped image still gonna be drawn in position `x` and `y`, but extended backwards the positive sides of x- and y-axis, so it's towards left and up
	* it flips along the axis
* since `y` is where the upper edge of the pipe starts, without adding the `PIPE_HEIGHT`, and with the flipped pipe image extend towards up, the whole pipe is outside of the top side of the screen
	* so `PIPE_HEIGHT` is there for shifting the pipe down, bringing it back into the screen
* for the lower pipe we don't need to handle y's value as it's taken care by `PipePair`

* for the x and y scale, `-1` means flip it or mirror it
___

> [!quote]
> Procedural generation, ultimately, is taking and manipulating values that we used to construct our scenes with, typically randomly.
> 
> `math.random()` some value. That's how to make random levels, in a nutshell.
> Making good random levels though, is another question.

___
