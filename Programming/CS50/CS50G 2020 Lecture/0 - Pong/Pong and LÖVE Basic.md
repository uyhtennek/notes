**some story**
3D Game Programming All In One
by Kenneth Finney
* Torque, a game engine popular in the late 2000s
* used a language called TorqueScript

**Goal: Pong**, released in 1972
![](https://upload.wikimedia.org/wikipedia/commons/2/26/Pong.svg)
___

# What is Lua?

* Portuguese for "moon"
* invented in 1993 in Brazil
* primarily a config language and a runtime language for compiled code bases
	* long story short, compiling code used to be very slow in the 90s
		* maybe minutes, or even hours
	* Lua makes it much faster and easier

* in Lua almost everything, except basic variables, are "tables"
	* it's essentially a dictionary in Python
	* or an object in JavaScript
* therefore Lua is similar to Python, or perhaps more so to JavaScript

* very popular in the video game industry
	* because game engines are a perfect example of code bases that are traditionally compiled code for better performance
	* but it can be very cumbersome to add minor functionality, and then recompile it
		* and then the it take hours
* it is intended as a config language
	* long story short, that makes it excellent for storing data as well as code
	* *data-driven design*
___

# What is LÖVE 2D?

* a fast 2D game development framework
* compiled in C++
	* it runs very efficiently, as it's simple
* despite the fact that we're running it in Lua

* contains modules for 2D game development (officially)
	* graphics, keyboard input, math, audio, windowing, physics, etc.
	* some people working on 3D experiments though

* completely free
* portable
	* can run on mobile and the web

* Great for prototyping!
	* fast to whip something up in LÖVE 2D
	* then port that over to whatever framework or engine
___

# Game Loop

![game-loop-fixed.png (1040×412) (gameprogrammingpatterns.com)](https://gameprogrammingpatterns.com/images/game-loop-fixed.png)

an game fundamentally is just an infinite loop
* but every iteration of the loop has 3 major steps

1. process input
* the player pressed a key on the keyboard, or touched the joystick, or moved the mouse
* or player didn't input anything

2. we need to feed the input into the update
* change anything in our game state that relies upon the player's inputs
* eg. we should move our paddles, detect collision, etc.

3. we register all changes in game state, and we need to re-render that
* so players can see the result of their interactions
___

# 2D Coordinate System
a 2D world is essentially just x and y-axis
* yeah, geometry

in this case though, zero is top-left corner, but not bottom-left
* x still is x negative left, positive right
* y is y negative up, positive down
___

# Important Functions

LÖVE 2D expects **just** a `main.lua` file,
and will run the `main.lua` file
* but we can reference any other file within the directory from `main.lua`
* so `main.lua` is like a bootstrap, connecting the files we need


## pong-0: `love`

`love.load()`
* execute at the very beginning of the application
	* a startup function
* we can also define all the behavior outside of the function above it
	* but it's good practice to find it within `love.load()`
	* so it's easier to find all the startup code

`love.update(dt)`
* called each frame, and passed it `dt` each frame
* `dt` is the elapsed time in seconds since the last frame
	* typically one-sixtieth of a second
	* we use this to scale changes in the game to achieve even behavior across all frame rates
* eg. change paddles' positions, etc.

`love.draw()`
* called each frame, after update
* we define all of our drawing behavior, or rendering behavior here
	* eg. draw the paddles, draw the ball, etc.

`love.graphics.print(text, x, y, [width], [align])`
* similar to `printf()` in C
	* but this print goes onto the screen, instead of the terminal
* `text` - a string
* `x,y` - corrdinate
* `width` and `align` are optional
	* `width` is how much to align it
	* `align` is the mode of alignment
* eg. `love.graphics.print("Hello", 0, 0, WINDOW_WIDTH, 'center')`

`love.window.setMode(width, height, params)`
* initialize the window's dimensions, and set parameters
	* so it sets up our window
* `params` for *parameters*, it's a table
	* optional
	* eg. vsync, fullscreen, whether the window is resizable
* V-sync makes the game synced to our monitor's refresh rate
	* short for *vertical sync*

```lua
WINDOW_WIDTH = 1280
WINDOW_HEIGHT = 720

function love.load()
	love.window.setMode(WINDOW_WIDTH, WINDOW_HEIGHT, {
		fullscreen = false,
		resizable = false,
		vsync = true
	})
end

function love.draw()
	love.graphics.printf(
	'Hello Pong!',
	0,
	-- default font size is 12 pixels tall
	-- we do -6 here to make the text perfectly centered vertically in the screen
	WINDOW_HEIGHT / 2 - 6,
	WINDOW_WIDTH,
	'center'
	)
end
```


## pong-1: texture filtering

`love.graphics.setDefaultFilter(min, mag)`
* apply a texture scaling filter to the fonts or images in the game
	* a filter gets applied whenever we magnify or downscale a texture
* by default, it'd be a bilinear filter
	* it makes the textures look slightly blurred, so it doesn't look too pixilated
* good for high-res 2D game development, but it's not what we need
	* since we're making Pong, we need a retro style, we need a very cris, pixilated look
	* we want nearest-neighbor filtering (`'Nearest Neighbor'` or `'nearest'` or `'point'`)

![19709-spritefilter.png (515×429) (unity.com)](https://answers.unity.com/storage/temp/19709-spritefilter.png)

```lua
-- https://github.com/Ulydev/push
push = require 'push'

WINDOW_WIDTH = 1280
WINDOW_HEIGHT = 720

VIRTUAL_WIDTH = 432
VIRTUAL_HEIGHT = 243

function love.load()
	-- with 'push' library we can think of the game in 432x243
	-- but still render it in 1280x720
	push:setupScreen(VIRTUAL_WIDTH, VIRTUAL_HEIGHT, WINDOW_WIDTH, WINDOW_HEIGHT, {
		fullscreen = false,
		resizable = false,
		vsync = true
	})
end

function love.draw()
	-- similar in spirit to how OpenGo programming works
	push:apply('start')
	
	love.graphics.printf('Hello Pong!', 0, VIRTUAL_HEIGHT / 2 - 6, VIRTUAL_WIDTH, 'center')
	
	push:apply('end')
end
```

`love.keypressed(key)`
* a callback function that LÖVE expect, again, in `main.lua`
	* in the same vein as `love.load()`, `love.update(dt)`, `love.draw()`
* gets called every time by LÖVE 2D whenever we press a key
* `key` is a string

```lua
function love.keypressed(key)
	if key == 'escape' then
		love.event.quit()
	end
end
```

`love.event.quit()`
* simply terminate the application


## pong-2: rectangle

`love.graphics.newFont(path, size)`
* default font is Arial
* loads a font file into memory at a specific path
* and sets it to a specific size
	* we need to set the font size here because font objects are immutable
	* if we want another text with the same font but different size, we need to load once more

`love.graphics.setFont(font)`
* takes a font object instantiated with `love.graphics.newFont(path, size)`
* set the active font in LÖVE 2D, to be the `font`
* then the following prints will use that active font
* similarly there is a LÖVE 2D's active color

`love.graphics.clear(r, g, b, a)`
* flush the screen with the color defined by `(r, g, b, a)`
	* values are 0-1
	* in older version of löve 2D they are 0-255 though
* useful for drawing flat color backgrounds

`love.graphics.rectangle(mode, x, y, width, height)`
* `mode` is either `'fill'` or `'line'`

```lua
function love.load()
	love.graphics.setDefaultFilter('nearest', 'nearest')
	
	smallFont = love.graphics.newFont('font.ttf', 8)
	love.graphics.setFont(smallFont)
	
	push:setupScreen(VIRTUAL_WIDTH, VIRTUAL_HEIGHT, WINDOW_WIDTH, WINDOW_HEIGHT, {
		fullscreen = false,
		resizable = false,
		vsync = true
	})
end

function love.draw()
	push:apply('start')
	
	love.graphics.clear(40, 45, 52, 255)
	
	love.graphics.printf('Hello Pong!', 0, 20, VIRTUAL_WIDTH, 'center')
	
	love.graphics.rectangle('fill', 10, 30, 5, 20)
	love.graphics.rectangle('fill', VIRTUAL_WIDTH - 10, VIRTUAL_HEIGHT - 50, 5, 20)
	love.graphics.rectangle('fill', VIRTUAL_WIDTH / 2 - 2, VIRTUAL_HEIGHT / 2 - 2, 4, 4)
	
	push:apply('end')
end
```


## pong-3: input

`love.keyboard.isDown(key)`
* returns `true` or `false`
	* is the `key` is currently held down
	* so unlike `love.keypressed(key)`, this will keep returning `true` if the key keep being pressed
		* `love.keypressed(key)` just fires once when the key is initially pressed down
* since we want the paddles to keep moving while we're pressing the key, we have to use this one but not `love.keypressed(key)`

```lua
...
PADDLE_SPEED = 200

function love.load()
	smallFont = love.graphics.newFont('font.tff', 8)
	scoreFont = love.graphics.newFont('font.tff', 32) -- same font, but different size
	love.graphics.setFont(smallFont)
	...
	player1Score = 0
	player2Score = 0
	
	player1Y = 30
	player2Y = VIRTUAL_HEIGHT - 50
end

function love.update(dt)
	-- player 1 movement
	if love.keyboard.isDown('w') then
		player1Y = player1Y + -PADDLE_SPEED * dt
	elseif love.keyboard.isDown('s') then
		player1Y = player1Y + PADDLE_SPEED * dt
	end
	
	-- player 2 movement
	if love.keyboard.isDown('up') then
		player2Y = player2Y + -PADDLE_SPEED * dt
	elseif love.keyboard.isDown('down') then
		player2Y = player2Y + PADDLE_SPEED * dt
	end
end

function love.draw()
	...
	love.graphics.setFont(scoreFont)
	love.graphics.print(tostring(player1Score), VIRTUAL_WIDTH / 2 - 50,
		VIRTUAL_HEIGHT / 3)
	love.graphics.print(tostring(player2Score), VIRTUAL_WIDTH / 2 - 50,
		VIRTUAL_HEIGHT / 3)
	...
end
```


## pong-4: random

random generation is very common in game
* so that we get unpredictability and variability between different instances of our games

`math.randomseed(num)`
* no `love` this time, so it's a Lua thing, not a LÖVE 2D thing
* "Seed" is a value to base all of the random numbers, generated thereafter, off of
* so `num` is a starting number, then some mathematical operations are performed, to derive some new random values that we can use
	* therefore if `num` never change, the generated random values are always the same as well
	* which means it's not random at all, it's consistent

`os.time()`
* it's common to pass the current Unix epoch time, in seconds, as the seed
	* because it's gonna be different every time we run the game, no matter what
	* Unix epoch time is the time since `00:00:00 UTC, Januaray 1, 1970`
* Lua uses this too and we can get it using this function

`math.random(min, max)`
* returns a random number, inclusively between `min` and `max`
	* eg. `math.random(50)` returns 1 - 50
* `min` is optionally, if there is no `min` it would be 1

`math.min(num1, num2)`
`math.max(num1, num2)`
* returns the lesser or the greater of the 2 numbers passed in

```lua
function love.load()
	...
	math.randomseed(os.time())
	...
	ballX = VIRTUAL_WIDTH / 2 - 2
	ballY = VIRTUAL_HEIGHT / 2 - 2
	
	-- dx, dy stands for delta-x and delta-y, commonly used to represent velocity
	-- in C it'd be like math.random(2) == 1 ? 100 : -100, but in Lua we use and or
	ballDX = math.random(2) == 1 and 100 or -100
	ballDY = math.random(-50, 50)
	
	gameState = 'start' -- when the game started we'll change it to 'play'
end

function love.update(dt)
	-- player 1 movement
	if love.keyboard.isDown('w') then
		player1Y = math.max(0, player1Y + -PADDLE_SPEED * dt)
	elseif love.keyboard.isDown('s') then
		player1Y = math.min(VIRTUAL_HEIGHT - 20, player1Y + PADDLE_SPEED * dt)
	end
	
	-- player 2 movement
	if love.keyboard.isDown('up') then
		player2Y = math.max(0, player2Y + -PADDLE_SPEED * dt)
	elseif love.keyboard.isDown('down') then
		player2Y = math.min(VIRTUAL_HEIGHT - 20, player2Y + PADDLE_SPEED * dt)
	end
	
	if gameState == 'play' then
		-- somehow there isn't a += operator
		ballX = ballX + ballDX * dt
		ballY = ballY + ballDY * dt
	end
end

function love.draw()
	...
	love.graphics.rectangle('fill', ballX, ballY, 4, 4)
	...
end

function love.keypressed(key)
	if key == 'escape' then
		love.event.quit()
	elseif key == 'enter' or key == 'return' then
		if gameState == 'start' then
			gameState = 'play'
		else
			gameState = 'start'
			
			ballX = VIRTUAL_WIDTH / 2 - 2
			ballY = VIRTUAL_HEIGHT / 2 - 2
			
			ballDX = math.random(2) == 1 and 100 or -100
			ballDY = math.random(-50, 50) * 1.5
		end
	end
end
```
___

# What is a class?

![604px-CPT-OOP-objects_and_classes_-_attmeth.svg.png (604×480) (wikimedia.org)](https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/CPT-OOP-objects_and_classes_-_attmeth.svg/604px-CPT-OOP-objects_and_classes_-_attmeth.svg.png)

* blueprints for creating bundles of data and code that are related
* **attributes** in a class describe the instance
	* eg. a car's brand, model, color, miles, etc.
	* also known as *fields*
* class can have **methods** that define its behavior
	* eg. a car can accelerate, turn, honk, etc.

* Objects are instances of a class
	* like we have a blueprint of a car, we need to take that to a factory to actually produce a car

* it's common to capitalize class names
	* to differentiate classes from concrete objects or variables or functions
	* also we would name the class files as `Ball.lua`, `Paddle.lua`, and the like
		* to differentiate from important files like `main.lua`
___

## pong-5: Lua's Class

* Lua's way of doing object oriented programming is a little bit different
* some folks put up a library to make Lua's classes much easier to implement
	* and much similar to Java or C# or Python

```lua
--! file: Ball.lua
Ball = Class{} -- so there is a Class table containing Ball Objects

function Ball:init(x, y, width, height)
	-- `self`, or `this` in other languages, means the instantiated object
	-- so `self` is an Object of Class Ball
	self.x = x
	self.y = y
	self.width = width
	self.height = height
	
	self.dy = math.random(2) == 1 and -100 or 100
	self.dx = math.random(-50, 50)
end

function Ball:reset()
	self.x = VIRTUAL_WIDTH / 2 - 2
	self.y = VIRTUAL_HEIGHT / 2 - 2
	self.dy = math.random(2) == 1 and -100 or 100
	self.dx = math.random(-50, 50)
end

--[[
	in main.lua, instead of putting all the code in love.update(dt) and love.draw()
	we will have the Objects to update and draw themselves
]]
function Ball:update(dt)
	self.x = self.x + self.dx * dt
	self.y = self.y + self.dy * dt
end

function Ball:render()
	love.graphics.rectangle('fill', self.x, self.y, self.width, self.height)
end
```

```lua
--! file: Paddle.lua
Paddle = Class{}

function Paddle:init(x, y, width, height)
	self.x = x
	self.y = y
	self.width = width
	self.height = height
	self.dy = 0
end

function Paddle:update(dt)
	if self.dy < 0 then
		self.y = math.max(0, self.y + self.dy * dt)
	else
		self.y = math.min(VIRTUAL_HEIGHT - self.height, self.y + self.dy * dt)
	end
end

function Paddle:render()
	love.graphics.rectangle('fill', self.x, self.y, self.width, self.height)
end
```

```lua
--! file: main.lua
...
-- https://github.com/vrld/hump/blob/master/class.lua
Class = require 'class'
require 'Paddle'
require 'Ball'
...
function love.load()
	...
	player1 = Paddle(10, 30, 5, 20)
	player2 = Paddle(VIRTUAL_WIDTH - 10, VIRTUAL_HEIGHT - 30, 5, 20)
	
	ball = Ball(VIRTUAL_WIDTH / 2 - 2, VIRTUAL_HEIGHT / 2 - 2, 4, 4)
	
	gameState = 'start'
end

function love.update(dt)
	...
	if gameState == 'play' then
		ball:update(dt)
	end
	
	player1:update(dt)
	player2:update(dt)
end

function love.keypressed(key)
	if key == 'escape' then
		love.event.quit()
	elseif key == 'enter' or key == 'return' then
		if gameState == 'start' then
			gameState = 'play'
		else
			gameState = 'start'
			ball:reset()
		end
	end
end

function love.draw()
	...
	-- later on for more complex games, we can put these in a loop
	-- to have the entities update() and render(), in a few lines of code
	player1:render()
	player2:render()
	ball:render()
	
	push:apply('end')
end
```


## pong-6: FPS

`love.window.setTitle(title)`
* sets the title of our application window
* by default it's just `'Untitled'`

`love.timer.getFPS()`
* returns a string which is the current FPS of our application
* can print it in the console or on screen

```lua
function love.load()
	...
	love.window.setTitle('Pong')
	...
end

function love.draw()
	...
	displayFPS()
	push:apply('end')
end

function displayFPS()
	love.graphics.setFont(smallFont)
	love.graphics.setColor(0, 255, 0, 255)
	-- be careful Lua doesn't allow string and number concatenation
	love.graphics.print('FPS: ' .. tostring(love.timer.getFPS()), 10, 10)
end
```
___

# AABB Collision Detection

* relies on all colliding entities to have *axis-aligned bounding boxes*
	* meaning the collision boxes didn't rotate, so completely aligned to x- and y-axis
* the idea is just, a collision happens when there is no edges of a box is outside the opposite edges of another box
	* so they are overlapping

```lua
if rect1.x is not > rect2.x + rect2.width and
   rect1.x + rect1.width is not < rect2.x and
   rect1.y is not > rect2.y + rect2.height and
   rect1.y + rect1.height is not < rect2.y:
	-- so there is a collision
	return true
else
	-- there isn't a collision
	return false
```
___

## pong-7: collision

```lua
--! file: Ball.lua
function Ball:collides(paddle)
	if self.x > paddle.x + paddle.width or paddle.x > self.x + self.width then
		return false
	end
	if self.y > paddle.y + paddle.height or paddle.y > self.y + self.height then
		return false
	end
	return true
end
```

```lua
--! file: main.lua
function love.update()
	if gameState == 'play' then
		-- collide with players
		if ball:collides(player1) then
			-- x velocity
			ball.dx = -ball.dx * 1.03
			ball.x = player1.x + player1.width -- move the ball outside of paddle
			
			-- so the ball continues going up or down
			-- but the angle would be randomized a bit
			if ball.dy < 0 then
				ball.dy = -math.random(10, 150)
			else
				ball.dy = math.random(10, 150)
			end
		end
		if ball:collides(player2) then
			-- x velocity
			ball.dx = -ball.dx * 1.03
			ball.x = player2.x - ball.width
			
			if ball.dy < 0 then
				ball.dy = -math.random(10, 150)
			else
				ball.dy = math.random(10, 150)
			end
		end
		
		-- collide with ceiling or floor
		if ball.y <= 0 then
			ball.y = 0
			ball.dy = -ball.dy
		end
		if ball.y >= VIRTUAL_HEIGHT - ball.height then
			ball.y = VIRTUAL_HEIGHT - ball.height
			ball.dy = -ball.dy
		end
	else
		...
	end
end
```


## pong-8: score

```lua
--! file: main.lua

function love.update()
	...
	if ball.x + ball.width < 0 then
		servingPlayer = 1
		player2Score = player2Score + 1
		ball:reset()
		gameState = 'serve'
	end
	
	if ball.x > VIRTUAL_WIDTH then
		servingPlayer = 2
		player1Score = player1Score + 1
		ball:reset()
		gameState = 'serve'
	end
	...
end
```
___

# State Machine

here are some different states of the character Mario
![state-flowchart.png (1040×667) (gameprogrammingpatterns.com)|500](http://gameprogrammingpatterns.com/images/state-flowchart.png)

a state machine essentially is just about
* how can we monitor what state we're in?
* what transitions take place between states?

* since each individual state has its own logic
* we want to break up the logic of these states separately
	* so we can scale our code much bigger
___

## pong-9: states

who ever lose, who should server the ball next round
* so how's the state machine of our game gonna be?

Serve state > Play state > a player loses, back to Serve state and get to serve the ball
* although in current example our state is just a string variable

```lua
--! file: main.lua

function love.update(dt)
	if gameState == 'serve' then
		ball.dy = math.random(-50, 50)
		if servingPlayer == 1 then
			ball.dx = math.random(140, 200)
		else
			ball.dx = -math.random(140, 200)
		end
	else if gameState == 'play' then
		...
	end
	
	if ball.x + ball.width < 0 then
		servingPlayer = 1
		player2Score = player2Score + 1
		ball:reset()
		gameState = 'serve'
	end
	
	if ball.x > VIRTUAL_WIDTH then
		servingPlayer = 2
		player1Score = player1Score + 1
		ball:reset()
		gameState = 'serve'
	end
	...
end
```

so for now we're just putting all the states in one `love.update(dt)` function
but later we'll see how we can implement these a little bit more abstractly


## pong-10: game loop

```lua
function love.update(dt)
	...
	if ball.x + ball.width < 0 then
		servingPlayer = 1
		player2Score = player2Score + 1
		
		if player2Score == 10 then
			winningPlayer = 2
			gameState = 'done'
		else
			gameState = 'serve'
			ball:reset()
		end
	end
	if ball.x > VIRTUAL_WIDTH then
		servingPlayer = 2
		player1Score = player1Score + 1
		
		if player1Score == 10 then
			winningPlayer = 1
			gameState = 'done'
		else
			gameState = 'serve'
			ball:reset()
		end
	end
	...
end

function love.keypressed(key)
	if key == 'escape' then
		love.event.quit()
	elseif key == 'enter' or key == 'return' then
		if gameState == 'start' then
			gameState = 'serve'
		elseif gameState == 'serve' then
			gameState = 'play'
		elseif gameState == 'done' then
			-- reset the whole game
			gameState = 'serve'
			
			ball:reset()
			
			player1Score = 0
			player2Score = 0
			
			if winningPlayer == 1 then
				servingPlayer = 2
			else
				servingPlayer = 1
			end
		end
	end
end

function love.draw()
	if gameState == 'start' then
		...
	elseif gameState =='serve' then
		...
	elseif gameState == 'play' then
		...
	elseif gameState == 'done' then
		love.graphics.setFont(largeFont)
		love.graphics.printf('Player ' .. tostring(winningPlayer) .. ' wins!',
			0, 10, VIRTUAL_WIDTH, 'center')
		love.graphics.setFont(smallFont)
		love.graphics.printf('Press Enter to restart!', 0, 30, VIRTUAL_WIDTH, 'center')
	end
	...
end
```
___

# bfxr
or **sfxr** for Linux
https://www.bfxr.net/

* simple program for generating sound


## pong-11: audio

`love.audio.newSource(path, [type])`
* creates an audio object that we can play back at any point
* `type` is optional, either be `"static"` or `"stream"`
	* streamed assets will be streamed from disk as needed
		* for larger sound effects and music tracks, streaming is more memory-effective
	* static assets will be preserved in memory
		* for small sound effects, they won't take up much memory at all, so static it is

```lua
function love.load()
	...
	sounds = {
		-- avoid putting spaces in the keys
		['paddle_hit'] = love.audio.newSource('sounds/paddle_hit.wav', 'static')
		['score'] = love.audio.newSource('sounds/score.wav', 'static')
		['wall_hit'] = love.audio.newSource('sounds/wall_hit.wav', 'static')
	}
	...
end

function love.update()
	...
	-- when collisions happen or scored, do something like
	sounds['paddle_hit']:play()
	-- there is an option to loop the sound forever, but we don't need it here
end
```

* we prefer the Pythonic way, `sounds['paddle_hit']`
	* because this way we can dynamically generate a lookup of our table
	* but we can't do this using `sounds.paddle_hit`

idk if I understand correctly...
* we can't do `for i in sounds` then `sounds.i`
* but we can do `for i in sounds` then `sounds[i]`


## pong-12: resize

`love.resize(width, height)`
* this is called every time we resize the application

```lua
function love.load()
	...
	push:setupScreen(..., {
		fullscreen = false
		resizable = true
		vsync = true
	})
	...
end

function love.resize(w, h)
	push:resize(w, h)
end
```

`push` underneath the hood takes a texture and renders the game to it,
then upscaled it to fill the game window
* we need to `resize()` it to make it works with the new dimension of the window

`push` also adds things like *letterboxing*
* useful if we want to maintain the exact same aspect ratio
___
