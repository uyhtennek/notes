# Match 3

* originated a little earlier than 2001
* a genre staple was Bejeweled
	* a web browser game

![Bejeweled_deluxe_sc1.jpg (365×273) (wikimedia.org)](https://upload.wikimedia.org/wikipedia/en/0/05/Bejeweled_deluxe_sc1.jpg)

* match 3 or more of the same shapes or jewels
* then they will disappear and we can gain score from that
	* the blocks above them will fall down like via gravity

![Candy_Crush_Saga_game_setup_example.jpg (258×387) (wikimedia.org)](https://upload.wikimedia.org/wikipedia/en/f/f4/Candy_Crush_Saga_game_setup_example.jpg)

Candy Crush is the big hit, coming from the Match 3 genre
* around 2012, 2013
___

# timer-0: basic timer
a very basic timer

```lua
function love.load()
	currentSecond = 0
	secondTimer = 0
end

function love.update(dt)
	secondTimer = secondTimer + dt
	
	if secondTimer > 1 then
		currentSecond = currentSecond + 1
		secondTimer = secondTimer % 1
	end
end

function love.draw()
	love.graphics.printf('Timer: ' .. tostring(currentSecond) .. ' seconds.', 0, 0, VIRTUAL_WIDTH, VIRTUAL_HEIGHT / 2, 'center')
end
```

`secondTimer = secondTimer % 1`
* other languages usually don't allow modulo floats, but Lua does
	* because in Lua there is no ints or floats, just numbers data type
* it gives the floating point values left over
___

## timer-1: also ugly timer

if we apply the basic timer, to 5 different variables
* we would need 5 timers
* it's hard to keep track of

```lua
function love.load()
	currentSecond1 = 0
	secondTimer1 = 0
	currentSecond2 = 0
	secondTimer2 = 0
	...
end

function love.update(dt)
	secondTimer1 = seoncdTimer1 + dt
	-- timer 1 increments 1 every 1 second
	if secondTimer1 > 1 then
		currentSecond1 = currentSecond1 + 1
		secondTimer1 = secondTimer 1 % 1
	end
	
	secondTimer2 = secondTimer2 + dt
	-- timer 2 increments 2 every 2 seconds
	if secondTimer2 > 2 then
		currentSecond2 = currentSecond2 + 1
		secondTimer2 = secondTimer2 % 2
	end
	...
end
```
___

# timer-2: better timer

```lua
Timer = requre 'knife.timer'

function love.load()
	intervals = {1, 2, 4, 3, 2}
	counters = {0, 0, 0, 0, 0}
	
	for i = 1, 5 do
		Timer.every(intervals[i], function()
			counters[i] = counters[i] + 1
		end)
	end
end
```

`Timer.every(interval, callback)`
* calls `callback` every `interval`
	* where `interval` is a length of time, in seconds

`Timer.after(interval, callback)`
* calls `callback` after `interval`

we like this because it's iterative and scalable
___

# tween-0: basic tween
eg. how do we move something from the left to the right, in 2 seconds?

```lua
MOVE_DURATION = 2

function love.load()
	flappySprite = love.graphics.newImage('flappy.png')
	
	-- assigning 2 variables at once, a Lua trick
	flappyX, flappyY = 0, VIRTUAL_HEIGHT / 2 - 8
	endX = VIRTUAL_WIDTH - flappySprite:getWidth()
	
	timer = 0
end

function love.update(dt)
	if timer < MOVE_DURATION then
		timer = timer + dt
		-- tween is essentially ratio
		flappyX = math.min(endX, endX * (timer / MOVE_DURATION))
	end
end

function love.draw()
	love.graphics.draw(flappySprite, flappyX, flappyY)
	love.graphics.print(tostring(timer), 4, 4)
end
```

again, imagine scaling up this to a 1,000 things
* that we need to keep track of
* maybe they move at different speed, or stop at different `endX`
___

# tween-1: better tween

```lua
TIMER_MAX = 10

function love.load()
	flappySprite = love.graphics.newImage('flappy.png')
	
	birds = {}
	
	for i = 1, 10000 do
		table.insert(birds, {
			x = 0,
			y = math.random(VIRTUAL_HEIGHT - 24),
			rate = math.random() + math.random(TIMER_MAX - 1)
		})
	end
end

function love.update(dt)
	if timer < TIMER_MAX then
		timer = timer + dt
		for k, bird in pairs(birds) do
			bird.x = math.min(endX, endX * (timer / bird.rate))
		end
	end
end

function love.draw()
	for k, bird in pairs(birds) do
		love.graphics.draw(flappySprite, bird.x, bird.y)
	end
	love.graphics.print(tostring(timer), 4, 4)
	-- let's do a stress test as well
	love.graphics.print('FPS: ' .. tostring(love.timer.getFPS()), 4, VIRTUAL_HEIGHT - 16)
end
```

`math.random()` rules
* `math.random()` gives a value between 0 and 0.9999999
* `math.random(10, 50)` instead, always give integer values
	* `math.random(10.0, 50.0)` gives integer values as well
	* `math.random(TIMER_MAX - 1)`, also gives integer values
		* 1 to 9 in this case
* so use `math.random() + math.random(TIMER_MAX - 1)` to get floating point numbers
___

# Knife
[airstruck/knife: A collection of useful micro-modules for Lua. (github.com)](https://github.com/airstruck/knife)

* better to be familiar with it if we stick around Löve 2D
___

## tween-2: `Timer.tween()`

```lua
function love.load()
	for i = 1, 1000 do
		table.insert(birds, {
			x = 0,
			y = math.random(VIRTUAL_HEIGHT - 24),
			rate = math.random() + math.random(TIMER_MAX - 1),
			opacity = 0 -- just this field is new
		})
	end
	
	endX = VIRTUAL_WIDTH - flappySprite:getWidth()
	
	for k, bird in pairs(birds) do
		Timer.tween(bird.rate, {
			[bird] = { x = endX, opacity = 255 }
		})
	end
end
...
function love.draw()
	for k, bird in pairs(birds) do
		-- 255, 255, 255 to not tint the image
		-- 0, 0, 0 would make the image goes all black
		love.graphics.setColor(255, 255, 255, bird.opacity)
		love.graphics.draw(flappySprite, bird.x, bird.y)
	end
end
```

`Timer.tween(duration, definition)`
* interpolates values specified in the `definition` table over the `duration`
* by the end the time gets to `duration`, the values of the variables specified will get to just like the table in `definition`
	* it's the final values of the transformation

* notice we now can tween both the `endX` and `opacity` at once

a little more advanced example
* used for making a simple scene transition on start game
```lua
transitionAlpha = 0
...
function StartState:update(dt)
	...
	if love.keyboard.wasPressed('enter') or love.keyboard.wasPressed('return') then
		if self.currentMenuItem == 1 then
			-- selected start game
			Timer.tween(1, {
				[self] = { transitionAlpha = 255 }
			}):finish(function()
				gStateMachine:change('begin-game', {
					level = 1
				})
				
				self.colorTimer:remove()
			end)
		else
			-- selected quit game
			love.event.quit()
		end
	end
end
...
function StartState:render()
	...
	love.graphics.setColor(255, 255, 255, self.transitionAlpha)
	love.graphics.rectangle('fill', 0, 0, VIRTUAL_WIDTH, VIRTUAL_HEIGHT)
end
```

`Timer.tween(duration, definition):finish(callback)`
* when the tween finished, execute `callback`

`self.colorTimer:remove()`
* we don't need this timer any more, so we removed it
___

# Chaining

eg. think of maybe a RPG game
* during a cutscene, a NPC walk up to our character
	* the path is predestined, very discrete, not random
* then it do some actions, maybe some gestures, at the same time pull up a dialog
* and then it turns around, and starts walking away

these sort of chaining, actually is relevant to timing things
* at least usually a lot of those happen over the course of time
* eg. after 5 seconds, NPC1 will walk up north, then turn left and say something

we won't do it like
* if NPC1 is at this tile, then do this. if NPC.dialogueOpen, then do that.

we want it to be like
* walk here > do this > do this > walk there > ...
* easy sequence of steps
___

## chain-0: basic chain

how to make a thing runs around the screen?
* touch each corner of the screen

eg. we want it to move
* top-left to top-right > top-right to bottom-right > bottom-right to bottom-left > bottom-left to top-left

```lua
MOVEMENT_TIME = 2

function love.load()
	...
	timer = 0
	
	destinations = {
		[1] = {x = VIRTUAL_WIDTH - flappySprite:getWidth(), y = 0},
		[2] = {x = VIRTUAL_WIDTH - flappySprite:getWidth(), y = VIRTUAL_HEIGHT - flappySprite:getHeight()},
		[3] = {x = 0, y = VIRTUAL_HEIGHT - flappySprite:getHeight()},
		[4] = {x = 0, y = 0}
	}
	
	for k, destination in pairs(destinations) do
		destination.reached = false
	end
end

function love.update(dt)
	timer = math.min(MOVEMENT_TIME, timer + dt)
	
	for i, destination in ipairs(destinations) do
		if not destination.reached then
			flappyX, flappyY =
				-- we need to keep track of the start position of each section
				-- but still, it's a basic form of tweening
				baseX + (destination.x - baseX) * timer / MOVEMENT_TIME,
				baseY + (destination.y - baseY) * timer / MOVEMENT_TIME
			
			if timer == MOVEMENT_TIME then
				-- reached the destination
				destination.reached = true
				baseX, baseY = destination.x, destination.y
				timer = 0
			end
			
			-- don't calculate the rest of the sections
			break
		end
	end
end

function love.draw()
	love.graphics.draw(flappySprite, flappyX, flappyY)
end
```
___

## chain-1: `Timer.tween():finish()`
* actually `:finish()` can be applied to any `Timer` operation

```lua
function love.load()
	Timer.tween(MOVEMENT_TIME, {
		[flappy] = {x = VIRTUAL_WIDTH - flappySprite:getWidth(), y = 0}
	})
	:finish(function()
		Timer.tween(MOVEMENT_TIME, {
			[flappy] = {x = VIRTUAL_WIDTH - flappySprite:getWidth(), y = VIRTUAL_HEIGHT - flappySprite:getHeight()}
		})
		:finish(function()
			Timer.tween(MOVEMENT_TIME, {
				[flappy] = {x = 0, y = VIRTUAL_HEIGHT - flappySprite:getHeight()}
			})
			:finish(function()
				Timer.tween(MOVEMENT_TIME, {
					[flappy] = {x = 0, y = 0}
				})
			end)
		end)
	end)
end
```

`Timer:finish(callback)`
* a function we can call after any `Timer` function
	* `tween`, `every`, `after`, etc.
* calls `callback` once the function has completed

this is unscalable, but we don't care any way
* it's nested anonymous functions
* there's a term for it, *callback hell*

### knife.chain
there're ways to flatten the nested code

```lua
Chain(
	moveFlappy(x, y),
	moveFlappy(x2, y2),
	...
)
```

* play nice for maybe a cut scene system
	* or something that's very declarative
* and look nice and readable
___
