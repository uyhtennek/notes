# breakout-5: Hearts

```lua
--! file:src/states/StartState.lua
...
function StartState:update(dt)
	...
	if love.keyboard.wasPressed('enter') or love.keyboard.wasPressed('return') then
		gSounds['confirm']:play()
		
		if highlighted == 1 then
			gStateMachine:change('serve', {
				paddle = Paddle(1),
				bricks = LevelMaker.createMap(),
				health = 3,
				score = 0
			})
		end
	end
	...
end
```

notice we start passing in the current app state, to the next state
* it's a common paradigm in programming
	* common in web development with React as well
* rather than making all those variables global
	* this is cleaner, and more in control
* so we pass values from state to another state, the other state does something about them, then yet again pass them to another state

```lua
--! file: src/states/ServeState.lua
...
function ServeState:update(dt)
	...
	if love.keyboard.wasPressed('enter') or love.keyboard.wasPressed('return')
		gStateMachine:change('play', {
			paddle = self.paddle,
			bricks = self.bricks,
			health = self.health,
			score = self.score,
			ball = self.ball
		})
	end
	...
end
...
```

```lua
--! file: main.lua
...
function love.load()
	...
	gFrames = {
		['paddles'] = GenerateQuadsPaddles(gTextures['main']),
		['balls'] = GenerateQuadsBalls(gTextures['main']),
		['bricks'] = GenerateQuadsBricks(gTextures['main']),
		['hearts'] = GenerateQuads(gTextures['main']),
	}
	...
end
...

function renderHealth(health)
	local healthX = VIRTUAL_WIDTH - 100
	
	for i = 1, health do
		love.graphics.draw(gTextures['hearts'], gFrames['hearts'][1], healthX, 20)
		healthX = healthX + 11
	end
	
	for i = 1, 3 - health do
		love.graphics.draw(gTextures['hearts'], gFrames['hearts'][2], healthX, 20)
		healthX = healthX + 11
	end
end

function renderScore(score)
	love.graphics.setFont(gFont['small'])
	love.graphics.print('Score:', VIRTUAL_WIDTH - 60, 5)
	love.graphics.printf(tostring(score), VIRTUAL_WIDTH - 50, 5, 40, 'right')
end
```

although `health` and `score` are not global, we need to draw them across multiple states any way
* we make the functions for drawing them global
* `renderHealth(health)` and `renderScore(score)`

```lua
--! file: src/states/PlayState.lua
...
function PlayState:update(dt)
	...
	for k, brick in pairs(self.bricks) do
		if brick.inPlay and self.ball:collides(brick) then
			self.score = self.score + 10
			brick:hit()
			...
		end
	end
	
	if self.ball.y >= VIRTUAL_HEIGHT then
		-- the ball goes below the screen
		self.health = self.health - 1
		gSounds['hurt']:play()
		
		if self.health == 0 then
			gStateMachine:change('game-over', {
				score = self.score
			})
		else
			gStateMachine:change('serve', {
				paddle = self.paddle,
				bricks = self.bricks,
				health = self.health,
				score = self.score
			})
		end
	end
	...
end

function PlayState:render()
	...
	renderScore(self.score)
	renderHealth(self.health)
	...
end
```

```lua
--! file: src/states/GameOverState.lua
GameOverState = Class{__includes = BaseState}

function GameOverState:enter(params)
	-- it just need the score, nothing else
	self.score = params.score
end

function GameOverState:update(dt)
	if love.keyboard.wasPressed('enter') or love.keyboard.wasPressed('return')
		gStateMachine:change('start')
	end
	if love.keyboard.wasPressed('escape') then
		love.event.quit()
	end
end

function GameOverState:render()
	love.graphics.setFont(gFonts['large'])
	love.graphics.printf('GAME OVER', 0, VIRTUAL_HEIGHT / 3, VIRTUAL_WIDTH, 'center')
	love.graphics.setFont(gFonts['medium'])
	love.graphics.printf('Final Score: ' .. tostring(self.score), 0, VIRTUAL_HEIGHT,
		VIRTUAL_WIDTH, 'center')
	love.graphics.printf('Press Enter!', 0, VIRTUAL_HEIGHT - VIRTUAL_HEIGHT / 4,
		VIRTUAL_WIDTH, 'center')
end
```
___

# breakout-6: patterns generation

```lua
--! file: src/LevelMaker.lua
-- brick patterns
NONE = 1
SINGLE_PYRAMID = 2
MULTI_PYRAMID = 3

-- per-row patterns
SOLID = 1
ALTERNATE = 2
SKIP = 3
NONE = 4

LevelMaker = Class{}

function LevelMaker.createMap(level)
	local bricks = {}
	
	local numRows = math.random(1, 5)
	local numCols = math.random(7, 13)
	-- ensure it's an odd number, as even number leads to asymmetry
	numCols = numCols % 2 == 0 and (numCols + 1) or numCols
	
	-- every level, increment color
	-- every 5 levels, increment tier, and back to color 1
	local highestTier = math.min(3, math.floor(level / 5))
	local highestColor = math.min(5, level % 5 + 3)
	
	for y = 1, numRows do
		local skipPattern = math.random(1, 2) == 1 and true or false
		local alternatePattern = math.random(1, 2) == 1 and true or false
		
		local alternateColor1 = math.random(1, highestColor)
		local alternateColor2 = math.random(1, highestColor)
		local alternateTier1 = math.random(0, highestTier)
		local alternateTier2 = math.random(0, highestTier)
		
		local skipFlag = math.random(2) == 1 and true or false
		local alternateFlag = math.random(2) == 1 and true or false
		
		local solidColor = math.random(1, highestColor)
		local solidTier = math.random(0, highestTier)
		
		for x = 1, numCols do
			if skipPattern and skipFlag do
				...
			elseif ... do
				...
			elseif ... do
				...
			else
				...
			end
		end
	end
end
```

the code isn't too crazy
but the result could look awesome, almost like handcrafted
___

# breakout-7: brick tier

```lua
--! file: src/Brick.lua
...
function Brick:hit()
	...
	-- go down a color or a tier when hitted
	if self.tier > 0 then
		if self.color == 1 then
			self.tier = self.tier - 1
			self.color = 5
		else
			self.color = self.color - 1
		end
	else
		if self.color == 1 then
			self.inPlay = false
		else
			self.color = self.color - 1
		end
	end
	...
end
...
```

```lua
--! file: src/states/PlayState.lua
...
function love.update(dt)
	...
	for k, brick in pairs(self.bricks) do
		if brick.inPlay and self.ball:collides(brick) then
			self.score = self.score + (brick.tier * 200 + brick.color * 25)
			brick:hit()
			...
		end
	end
	...
end
...
```
___

# breakout-8: particle

`love.graphics.newParticleSystem(texture, particles)`
* takes in a particle texture
	* all particle system needs some sort of texture as their foundation
* and the maximum number of particles that it is allowed to emit

```lua
--! file: src/Bricks.lua
...
paletteColors = {
	-- using a color palette would look better than completely random
	-- btw retro games don't have many colors due to hardware limitation
	[1] = { ['r'] = 99, ['g'] = 155, ['b'] = 255 }, -- blue
	[2] = { ['r'] = 106, ['g'] = 190, ['b'] = 47 }, -- green
	[3] = { ['r'] = 217, ['g'] = 87, ['b'] = 99 }, -- red
	[4] = { ['r'] = 215, ['g'] = 123, ['b'] = 186 }, -- purple
	[5] = { ['r'] = 251, ['g'] = 242, ['b'] = 54 } -- gold
}

function Brick:init(x, y)
	...
	self.psystem = love.graphics.newParticleSystem(gTextures['particle'], 64)
	-- various behavior-determining functions for the particle system
	-- https://love2d.org/wiki/ParticleSystem
	self.psystem:setParticleLifetime(0.5, 1)
	self.psystem:setLinearAcceleration(-15, 0, 15, 80)
	self.psystem:setAreaSpread('normal', 10, 10)
end

function Brick:hit()
	self.psystem:setColors(
		-- initial color
		paletteColors[self.color].r,
		paletteColors[self.color].g,
		paletteColors[self.color].b,
		55 * (self.tier + 1),  -- alpha
		-- end color
		paletteColors[self.color].r,
		paletteColors[self.color].g,
		paletteColors[self.color].b,
		0                      -- alpha
	)
	self.psystem:emit(64)
	...
end
...
```

> [!tips]
> if you're doing your own sprite art, try to use fewer colors.

___
