# breakout-2: bounce
* the ball need to bounce off walls and bricks
* similar to the case in [[Pong and LÖVE Basic|Pong]]

```lua
--! file: src/Ball.lua
Ball = Class{}

function Ball:init(skin) -- it's nice that we can specify a skin to use
	self.width = 8
	self.height = 8
	
	self.dy = 0
	self.dx = 0
	
	self.skin = skin
end

function Ball:collides(target)
	-- a simple AABB collision detection
	if self.x > target.x + target.width or target.x > self.x + self.width then
		return false
	end
	if self.y > target.y + target.height or target.y > self.y + self.height then
		return false
	end
	return true
end

function Ball:reset()
	self.x = VIRTUAL_WIDTH / 2 - 2
	self.y = VIRTUAL_HEIGHT / 2 - 2
	self.dx = 0
	self.dy = 0
end

function Ball:update(dt)
	-- update position
	self.x = self.x + self.dx * dt
	self.y = self.y + self.dy * dt
	
	-- bounce back walls
	if self.x <= 0 then
		self.x = 0
		self.dx = -self.dx
		gSounds['wall-hit']:play()
	end
	if self.x >= VIRTUAL_WIDTH - 8 then
		self.x = VIRTUAL_WIDTH - 8
		self.dx = -self.dx
		gSounds['wall-hit']:play()
	end
	if self.y <= 0 then
		self.y = 0
		self.dy = -self.dy
		gSounds['wall-hit']:play()
	end
end

function Ball:render()
	love.graphics.draw(gTextures['main'], gFrames['balls'][self.skin], self.x, self.y)
end
```

```lua
--! file: main.lua
...
function love.load()
	...
	gFrames = {
		['paddles'] = GenerateQuadsPaddles(gTextures['main']),
		['balls'] = GenerateQuadsBalls(gTextures['main'])
	}
	...
end
...
```

```lua
--! file: src/Util.lua
...
function GenerateQuadsBalls(atlas)
	-- same concept to the paddle one
	local x = 96
	local y = 48
	
	local count = 1
	local quads = {}
	
	for i = 0, 3 do -- 4 here
		quads[counter] = love.graphics.newQuad(x, y, 8, 8, atlas:getDimensions())
		x = x + 8
		counter = counter + 1
	end
	
	x = 96
	y = 56
	
	for i = 0, 2 do -- 3 here
		quads[counter] = love.graphics.newQuad(x, y, 8, 8, atlas:getDimensions())
		x = x + 8
		counter = counter + 1
	end
	
	return quads
end
```

lastly we need to spawn the ball
```lua
--! file: src/states/PlayState.lua
...
function PlayState:init()
	self.paddle = Paddle()
	self.ball = Ball(math.random(7))
	
	self.ball.dx = math.random(-200, 200)
	self.ball.dy = math.random(-50, -60)
	
	self.ball.x = VIRTUAL_WIDTH / 2 - 4
	self.ball.y = VIRTUAL_HEIGHT - 42
end

function PlayState:update(dt)
	...
	self.paddle:update(dt)
	self.ball:update(dt)
	
	if self.ball:collides(self.paddle) then
		-- we assume if it collides with the paddle, it's coming down
		self.ball.dy = -self.ball.dy
		-- remember to reset position when ever using AABB collisions
		gSounds['paddle-hit']:play()
	end
	...
end
...
```
___

# breakout-3: bricks
just simple bricks, no advanced procedural generation yet

```lua
--! file: main.lua
...
function love.load()
	...
	gFrames = {
		['paddles'] = GenerateQuadsPaddles(gTextures['main']),
		['balls'] = GenerateQuadsBalls(gTextures['main']),
		['bricks'] = GenerateQuadsBricks(gTextures['main'])
	}
	...
end
...
```

```lua
--! file: src/Util.lua
...
function GenerateQuadsBricks(atlas)
	-- since bricks are all the same size
	return table.slice(GenerateQuads(atlas, 32, 16), 1, 21)
end
...
```

```lua
--! file: src/Brick.lua
Brick = Class{}

function Brick:init(x, y)
	self.tier = 0
	self.color = 1
	
	self.x = x
	self.y = y
	self.width = 32
	self.height = 16
	
	-- if it's in play, update and render it; if not, just ignore it.
	-- since we don't have many bricks, we don't really need to worry about memory
	self.inPlay = true
end

function Brick:hit()
	gSounds['brick-hit-2']:play()
	self.inPlay = false
end

function Brick:render()
	if self.inPlay then
		love.graphics.draw(gTextures['main'],
			-- since every color is seperated by 3 tiers, in the sprite sheet
			gFrames['bricks'][1 + ((self.color - 1) * 4) + self.tier],
			self.x, self.y)
	end
end
```

> [!tips]
> Lua has garbage collectors. Just almost identical to Java.
> Every table that no longer has reference to, Lua just clear them in whatever interval it's set.

```lua
--! file: src/states/PlayState.lua
...
function PlayState:init()
	...
	-- LevelMaker has all the logic about generating levels
	-- rather than have the logic in PlayState or VictoryState or the likes
	self.bricks = LevelMaker.createMap()
end

function PlayState:update(dt)
	...
	for k, brick in pairs(self.bricks) do
		if brick.inPlay and self.ball:collides(brick) then
			brick:hit()
		end
	end
	
	if love.keyboard.wasPressed('escape') then
		love.event.quit()
	end
end

function PlayerState:render()
	for k, brick in pairs(self.bricks) do
		brick:render()
	end
	...
end
```

```lua
--! file: src/LevelMaker.lua
LevelMaker = Class{}

function LevelMaker.createMap(level)
	local bricks = {}
	
	local numRows = math.random(1, 5)
	local numCols = math.random(7, 13)
	
	for y = 1, numRows do
		for x = 1, numCols do
			b = Brick(
				-- 32 - brick width; 8 - paddings on either side;
				-- (13 - numCols) * 16 - shift the bricks in a row, to center them
				(x-1) * 32 + 8 + (13 - numCols) * 16,
				y * 16
			)
			table.insert(bricks,b)
		end
	end
end
```
___

# breakout-4: collision

the ball bounces off of the brick, based on which side it hits
* if it hits the side, `ball.x` should be `-ball.x`, but `ball.y` remains unchanged
* if it hits the top or bottom, `ball.y` then become `-ball.y`, `ball.x` remains unchanged

but we don't know this just based off the collision
* currently we can just know whether the collision is true or not

also currently if the ball comes to the paddle by the side, it can get stucked on the paddle
* in a normal breakout it should bounce off of the paddle in a sharper angle or direction
	* based on how far away the collision happens from the center of the paddle, the ball's bounce off angle would be sharper or less sharp
* it's done by taking the distance and then amplifying the ball's delta X by that

1. Paddle Collision
* take `math.abs(paddle.x + paddle.width / 2 - ball.x)`
* scale that up a little bit, and apply it to `ball.dx`
	* also we should make `ball.dx` either positive or negative
	* based on whether the ball hits the left side or right side of the paddle

2. Brick Collision
* check which edge of the ball isn't inside of the brick
```pseudocode
if ball's left edge is outside brick and dx is positive:
	trigger left-side collision
	same y direction, but negate dx
elseif ball's right edge is outside brick and dx is negative:
	trigger right-side collision
	same y direction, dx is positive this time
elseif ball's top edge is outside brick:
	trigger top-side collsion
else:
	trigger bottom-side collision
```

* notice we just need to check either one side of the ball
* no need do:
	* if left edge is outside brick, and right edge is inside brick
	* if right edge is outside brick, and left edge is inside brick
* because we have `dx`, so we can shortcut that check

> [!tips]
> these collision checks are far from perfect, but it works at least 90% of the times
> for a more advanced and better solution, see:
> * https://github.com/noooway/love2d_arkanoid_tutorial
> * https://github.com/noooway/love2d_arkanoid_tutorial/wiki/Resolving-Collisions
> 	* take how much the x and the y differed on different points of the bricks
> 	* more robust

```lua
--! file: src/states/PlayState.lua
...
function PlayState:update(dt)
	...
	if self.ball:collides(self.paddle) then
		...
		local paddle_middle = self.paddle.x + self.paddle.width / 2
		if self.ball.x < paddle_middle and self.paddle.dx < 0 then
			self.ball.dx = -50 + -(8 * (paddle_middle - self.ball.x))
		elseif self.ball.x > paddle_middle and self.paddle.dx > 0 then
			self.ball.dx = 50 + (8 * math.abs(paddle_middle - self.ball.x))
		end
		
		gSounds['paddle-hit']:play()
	end
	
	for k, brick in pairs(self.bricks) do
		if brick.inPlay and self.ball:collides(brick) then
			brick:hit()
			
			if self.ball.x + 2 < brick.x and self.ball.dx > 0 then
				self.ball.dx = -self.ball.dx
				self.ball.x = brick.x - self.ball.width
			elseif self.ball.x + 6 > brick.x + brick.width  and self.ball.dx < 0 then
				self.ball.dx = -self.ball.dx
				self.ball.x = brick.x + brick.width
			elseif self.ball.y < brick.y then
				self.ball.dy = -self.ball.dy
				self.ball.y = brick.y - ball.height
			else
				self.ball.dy = -self.ball.dy
				self.ball.y = brick.y + brick.height
			end
			
			self.ball.dy = self.ball.dy * 1.02
			break
		end
	end
	...
end
...
```

notice we used `self.ball.x + 2` and `self.ball.x + 6`. why don't we just use `self.ball.x`?
* it plays a little bit rough with corners
	* the ball can come in at an angle, that it's intersecting with the brick in two positions
	* on top and on either left or right; on bottom and on either side.
* adding this 2 pixels padding, prioritizes the Y being hit

also it's common that if an object moves too fast, collision detections couldn't work well
* since the object just skips through other objects
	* the mathematics is correct but the behavior is not
* a more advanced solution would take a reverse approach
	1. on each frame, we start on where the ball is
	2. we add its width and height to itself
		* it uses a bunch of invisible balls, to bridge the gap
		* to see if any of those, collides with something
	3. before we apply `dx` or `dy` to the ball, we already know if it is gonna cause a collision
* it's more computationally expensive, but a lot more accurate
___
