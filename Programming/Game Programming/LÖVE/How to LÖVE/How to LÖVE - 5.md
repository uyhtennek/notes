# Debugging

here is a short game with a bug
```lua
function love.load()
	circle = {x = 100, y = 100}
	bullets = {}
end

function love.update(dt)
	for i,v in ipairs(bullets) do
		v.x = v.x + 400 * dt
	end
end

function love.draw()
	love.graphics.circle("fill", circle.x, circle.y, 50)
	for i,v in ipairs(bullets) do
		love.graphics.circle("fill", v.x, v.y, 10)
	end
end

function love.keypressed()
	if key == "space" then
		shoot()
	end
end

function shoot()
	table.insert(bullets, {circle.x, circle.y})
end
```
* the bug is it's supposed to shoot a bullet when `"space"` is pressed
	* but it doesn't work. there is no bullets appearing.

1. What are the possible reasons?
* it doesn't shoot the bullets
* it's shooting the bullets but it's not drawing them correctly
	* maybe they're in the wrong position

2. using `print()` to debug
* remember to add the following code at the top of `main.lua` though
* `io.stdout:setvbuf("no")`
	* to make the print message appear right away, and we don't need to close the game for it
* also, `print()` can take infinite amount of arguments

3. the first mistake is, `love.keypressed()` didn't have a `key` parameter
* it should be `love.keypressed(key)`
* but then it gives us another error when we try to hit `"space"` to shoot a bullet

### Reading errors
An error occurs when the code is trying to execute something that isn't possible

eg. multiply a number by a string
* `print(100 * "test")`
![error1.png (465×238) (sheepolution.com)](https://sheepolution.com/images/book/20/error1.png)

eg. call a function that doesn't exist
* `doesNotExist()`
![error2.png (460×248) (sheepolution.com)](https://sheepolution.com/images/book/20/error2.png)

4. this is the error our bullet shooting game currently getting
![error3.png (502×209) (sheepolution.com)](https://sheepolution.com/images/book/20/error3.png)

so the error message actually tells exactly why and where the error occurs
`main.lua:11: attempt to perform arithmetic on field 'x'(a nil value)`
* where? line 11.
	* `v.x = v.x + 400 * dt`
* why? it was trying to do calculation on `nil` value
	* *arithmetic* means a calculation, eg. using `+`, `-`, `*`, etc.
	* but `x` is a `nil` value

5. the mistake we made is, `table.insert(bullets, {circle.x, circle.y})` in `function shoot()`
* we didn't assign the fields `x` and `y` to the table `bullets`
	* we did assign them the values though

let's fix that
```lua
function shoot()
	table.insert(bullets, {x = circle.x, y = circle.y})
end
```

### another practice
```lua
Object = require "classic"

Circle = Object:extend()

function Circle:new()
	self.x = 100
	self.y = 100
end

function Circle:draw(offset)
	love.graphics.circle("fill", self.x + offset, self.y, 25)
end

function love.load()
	circle = Circle()
end

function love.draw()
	circle:draw(10)
	circle:draw(70)
	circle.draw(140)
	circle:draw(210)
end
```
![error4.png (439×218) (sheepolution.com)](https://sheepolution.com/images/book/20/error4.png)

`attempt to index` means it tried to find a property of something
* in this case it tried to find the property `x` on the variable `self`
* but the error said `self` is a number value

Why is that?
* line 11 is inside the function `Circle:draw(offset)`
	* we used colon `:` for `Circle:draw(offset)`
	* so it automatically has `self` as first parameter
* but apparently somewhere we passed a number value as the first argument `self`
	* instead of `offset`

We can find where it went wrong, by reading the Traceback
* the bottom part of the error tells us the [[C - 6 functions#call stack|call stacks]] before reaching the error
* it's read from down to top, the top one is the nearest function call
	* ignore the `xpcall` line

so before calling `Circle:draw(offset)`, it's called from `love.draw()`
* indeed on line 21, there is a `circle.draw(140)` instead of `circle:draw(140)`
___

## Syntax errors
A syntax error occurs when the program tries to "read" the code but can't understand it.
* because of mistakes about in the syntax, obviously

eg. if a variable name is started with a number, it gives this error
![error5.png (443×81) (sheepolution.com)](https://sheepolution.com/images/book/20/error5.png)

eg.
```lua
function love.load()
	timer = 0
	show = true
end

function love.update(dt)
	show = false
	timer = timer + dt
	
	if timer > 1 then
		if love.keyboard.isDown("space") then
			show = true
		end
	if timer > 2 then
		timer = 0
	end
end

function love.draw()
	if show then
		love.graphics.rectangle("fill", 100, 100, 100, 100)
	end
end
```
![error6.png (615×210) (sheepolution.com)](https://sheepolution.com/images/book/20/error6.png)

`<eof>` means *end of file*
* it expects an `end` at the end of the file
* but placing an `end` at the end of the file is not how we fix it
	* it says that it expects the `end` to close the function at line 6
	* so let's start from there and go down

And yes there is missing an `end`
* probably the incorrect indentation there made that mistake hard to be spotted

here is another common mistake
```lua
function love.load()
	t = {}
	table.insert(t, {x = 100, y = 50)
end
```
![error7.png (383×84) (sheepolution.com)](https://sheepolution.com/images/book/20/error7.png)

* it's missing a closing curly bracket
___

## Asking for help

1. [LÖVE forum](https://www.love2d.org/forums/index.php)
2. [LÖVE Game Framework Discord](https://discord.gg/R8kvdaM4)

remember to help the guys to help you
* what exactly goes wrong and what you expect to happen?
* explain what you have tried to fix it so far
* show the code
	* at least some of the code where you think it goes wrong
* share the `.love` file
	* so people can try it out, for debug

> [!remember]
> No one is obligated to help you, and the ones that do all do it for free, so be kind 🙂

___

## Rubber duck debugging
[Wikipedia](https://en.wikipedia.org/wiki/Rubber_duck_debugging)

the idea is that when you explain what you're doing in your code,
you often realize what you're doing wrong and fix the bug yourself

instead of explaining it to someone, maybe try explaining it to your rubber duck

meet Hendrik!
![hendrik.png (335×291) (sheepolution.com)](https://sheepolution.com/images/book/20/hendrik.png)
___

# Saving and Loading
actually it's more like writing and reading a file

1. here is a small game where players can pick up coins
* this is how the player looks ![face.png (33×27) (sheepolution.com)](https://sheepolution.com/images/book/21/face.png)
* this is how the coin looks ![dollar.png (10×16) (sheepolution.com)](https://sheepolution.com/images/book/21/dollar.png)
* if the distance between the player and a coin is close enough, pick up the coin
<center>
<img src="https://sheepolution.com/images/book/21/circles_collision.png">
</center>

```lua
function checkCollision(p1, p2)
	local distance = math.sqrt((p1.x - p2.x)^2 + (p1.y - p2.y)^2)
	return distance < p1.size + p2.size
end

function love.load()
	-- player object
	player = {
		x = 100,
		y = 100,
		size = 25
		image = love.graphics.newImage("face.png")
	}
	
	-- store coins
	coins = {}
	for i=1,25 do
		table.insert(coins,
			{
				-- give random position to the coins
				x = math.random(50, 650),
				y = math.random(50, 450),
				size = 10,
				image = love.graphics.newImage("dollar.png")
			}
		)
	end
end

function love.update(dt)
	-- move with keyboard
	if love.keyboard.isDown("left") then
		player.x = player.x - 200 * dt
	elseif love.keyboard.isDown("right") then
		player.x = player.x + 200 * dt
	end
	
	if love.keyboard.isDown("up") then
		player.y = player.y - 200 * dt
	elseif love.keyboard.isDown("down") then
		player.y = player.y + 200 * dt
	end
	
	-- iterate through all the coins to check if it's touching the player
	for i,v in ipairs(coins) do
		if checkCollision(player, v) then
			table.remove(coins, i) -- remove the coin
			player.size = player.size + 1 -- make the player grows in size
		end
	end
end

function love.draw()
	-- draw player
	love.graphics.circle("line", player.x, player.y, player.size)
	love.graphics.draw(player.image, player.x, player.y,
		0, 1, 1, player.image:getWidth()/2, player:getHeight()/2)
	
	-- draw coins
	for i,v in ipairs(coins) do
		love.graphics.circle("line", v.x, v.y, v.size)
		love.graphics.draw(v.image, v.x, v.y,
			0, 1, 1, v.image:getWidth()/2, v.image:getHeight()/2)
	end
end
```

![coin_grower.gif (645×398) (sheepolution.com)](https://sheepolution.com/images/book/21/coin_grower.gif)

2. before we go over to saving and loading, let's make a few more changes
* restart the game a few times, we can see that even through the circles are positioned quick randomly, they are always spawned on the same random spot
___

### random seed
we need to use `math.randomseed()`
* the random numbers we generate are actually not totally random, they're based on a number, which we call the seed
* if we don't change the seed, we always get the same random positions

although it's not totally random, there is an advantage
* we can save and share
* like the world seed in Minecraft
	* we can share our world seed to others, and others can get the same generated world

then what number do we use as our seed?
* what number is unique every time we start the game?
* one option being the `os.time()`
	* it's a Lua function that gives us the time of the operation system, to the second
	* so that's 86400 unique numbers a day
* another better option might be to use LÖVE math library
	* LÖVE's random number generated, *rng*, is automatically seeded, with `os.time()`
		* but somehow it overall is better or more random than Lua's rng
	* use `love.math.random()` instead of `math.random()`

```lua
function love.load()
{
	-- player object
	player = {
		...
	}
	
	-- store coins
	coins = {}
	for i=1, 25 do
		table.insert(coins,
			{
				x = love.math.random(50, 650),
				y = love.math.random(50, 450),
				size = 10,
				image = love.graphics.newImage("dollar.png")
			}
		)
	end
}
```
___

### removing items from a list
* another potential problem in the game is that, we have a for-loop in which we remove coins

think about it, we're iterating through a list, and we're removing items from that list
* when an item is removed, the list gets shorter
* it messes up the for-loop

let's see this in an example
```lua
local test = {"a", "b", "c", "d", "e", "f", "g", "h", "i", "j"}
for i,v in ipairs(test) do
	table.remove(test, i)
end
print(#test)
-- Output: 5
```

wait, what? why 5?
* isn't it supposed to remove all of them? it should be 0!
* only half of the list are removed

That's because it removes an item from the list
* everything gets moved to the side
* resulting in items get skipped over
![table_remove.png (442×258) (sheepolution.com)|400](https://sheepolution.com/images/book/21/table_remove.png)

the solution is, we should loop through the table in reverse
this way we'll never skip over any items when we have remove something
![table_remove_reverse.png (442×258) (sheepolution.com)|400](https://sheepolution.com/images/book/21/table_remove_reverse.png)

however, we can't use `ipairs()` if we need to do a loop backwards
```lua
function love.update(dt)
	-- move with keyboard
	...
	
	-- loop through all coins, but backwards
	for i=#coins,1,-1 do
		if checkCollision(player, coins[i]) then
			table.remove(coins, i)
			player.size = player.size + 1
		end
	end
end
```
___

## Saving

3. okay finally we can get to the saving part
* but how? is it a table?
* but then what information to save?

let's just save this 3 pieces of information
* the player's position
* his size
* positions of coins that haven't been picked up yet

create a function `saveGame()`
* where we store the important data in a table
```lua
function saveGame()
	data = {}
	data.player = {
		x = player.x,
		y = player.y,
		size = player.size
	}
	
	data.coins = {}
	for i,v in ipairs(coins) do
		-- 1.
		table.insert(data.coins, {x = v.x, y = v.y})
		-- 2.
		data.coins[i] = {x = v.x, y = v.y}
	end
end
```

first question, why save these specific parts, but not just the whole table?
* well, in general, don't use more data then you need
* we don't need to save the image width and height because they are always the same
* also, we can't save LÖVE objects
	* since both player and coin object has an image object
	* we can't save them

at this point we already have a table storing all the information we need to save
next we need to *serialize* it
* it means we need to make the table into something that we can read
* because right now if we print table, we can only get something like `table: 0x00e4ca20`
	* but that's not what we want to save

To serialize the table we're going to use a library
* [rxi/lume](https://github.com/rxi/lume)
* copy and paste `lume.lua` to our project
* it has all kinds of neat functions, but for this tutorial we just need `serialize` and `deserialize`

```lua
function love.load()
	lume = require "lume"
	...
end

function saveGame()
	data = {}
	data.player = {
		x = player.x,
		y = player.y,
		size = player.size
	}
	
	data.coins = {}
	for i,v in ipairs(coins) do
		data.coins[i] = {x = v.x, y = v.y}
	end
	
	serialized = lume.serialize(data)
	print(serialized)
end
...
```
* then the output this time will print the table in a way that we can read it
	* it's a string
	* that's what we'll be saving to our file

4. we can create, edit and read files with the [`love.filesystem`](https://www.love2d.org/wiki/love.filesystem) module
* [`love.filesystem.write(filename, data)`](https://www.love2d.org/wiki/love.filesystem.write) is the function for create/writing a file
	* the first argument is the name of the file
	* the second argument is the data that we want to write to the file

```lua
function saveGame()
	...
	serialized = lume.serialize(data)
	love.filesystem.write("savedata.txt", serialized)
end
```

* now to make that we can save. let's make it so that we can press F1 to save
```lua
function love.keypressed(key)
	if key == "f1" then
		saveGame()
	end
end
```

5. run and play the game, then press F1 to save
* but where is the save file?

* If you're on Windows it's saved in `AppData\Roaming\LOVE\`
* if we open the file we'll see our table is inside
___

## Loading

6. to load our data we need to
* check if a save file exists
* read the file
* turn the data into a table
* apply the data to our players and coins

so, first step, check if the file exists
* we can use `love.filesystem.getInfo(filename)` to do that
	* if the file exist, it returns a table with some information
	* if not, it returns `nil`
* since we don't need those extra information, we just gonna use it as a Boolean value

```lua
function love.load()
	lume = require "lume"
	
	player = {
		...
	}
	
	coins = {}
	
	if love.filesystem.getInfo("savedata.txt") then
		file = love.filesystem.read("savedata.txt")
		print(file)
	end
	
	for i=1,25 do
		table.insert(coins, { ... })
	end
end
```

now if we run the game, it'll print our save data
* since it's a string
next step, we need to turn this table string into a real table
* we gonna need `lume.deserialize()` for this one

```lua
function love.load()
	...
	if love.filesystem.getInfo("savedata.txt") then
		file = love.filesystem.read("savedata.txt")
		data = lume.deserialize(file)
	end
	...
end
```

lastly we need to apply the data to our player and coins
```lua
function love.load()
	...
	if love.filesystem.getInfo("savedata.txt") then
		file = love.filesystem.read("savedata.txt")
		data = lume.deserialize(file)
		
		-- apply save to player
		player.x = data.player.x
		player.y = data.player.y
		player.size = data.player.size
		
		-- apply save to coins
		for i,v in ipairs(data.coins) do
			coins[i] = {
				x = v.x,
				y = v.y,
				size = 10,
				image = love.graphics.newImage("dollar.png")
			}
		end
	else
		-- execute if don't have a save file
		for i=1,25 do
			table.insert(coins,
				{
					x = love.math.random(50, 650),
					y = love.math.random(50, 450),
					size = 10,
					image = love.graphics.newImage("dollar.png")
				}
			)
		end
	end
end
```
___

## Resetting
okay it can save and load. but what if we want to restart?
* delete save file, in game

we gonna need `love.filesystem.remove(filename)`
* let's make F2 as the key for deleting save and restarting game
* we can quit the game with `love.event.quit()`
	* pass `"restart"` as the first argument to make the game restart instead of quit

```lua
function love.keypressed(key)
	if key == "f1" then
		saveGame()
	elseif key == "f2" then
		love.filesystem.remove("savedata.txt")
		love.event.quit("restart")
	end
end
```
___

# Camera

when we have a big level that doesn't fit the screen, we use a "camera" that follows the player
* we need to move everything that we draw in some way
* so that the player is in the center of the screen

how to implement this?
we could have a variable like `camera_offset`, which is the distance between the player and the world's center or the point that our "camera" is looking at
* pass that to everything we're drawing
* `love.graphics.circle("line", player.x - camera.offset.x, player.y - camera.offset.y, player.size)`
* In other word, we just directly move the player to the center, along with everything

but if we have a very big game with a lot of things to draw, that's a lot of extra work
what we instead can do is use `love.graphics.translate()`
* it's part of the *Coordinate System*
* like how we can move, rotate, and scale an image
* we too can move, rotate, and scale the *canvas*
	* canvas is what we draw the things on

normally when we draw something on position `(x=0, y=0)`, it's the upper-left corner
but with `love.graphics.translate(400, 200)`, we moved the canvas
* so world's `(0,0)` will be the point `(400, 200)` on screen
* similarly, if we use `love.graphics.scale(2)`, then everything we draw will be scaled up
___

## Moving the canvas

![camera_center.gif (784×590) (sheepolution.com)|560](https://sheepolution.com/images/book/22/camera_center.gif)
this is what we're doing, by centering the player

1. we move the player to the world's center `(0,0)`
* since the player position is `(100,50)` in our example
	* somehow we don't like setting it as `(0,0)`
* we do this by `love.graphics.translate(-100,-50)`
	* that is to say, `love.graphics.translate(-player.x, -player.y)`

2. but we don't want the player ends up in the top-left of the screen, we want it in the center
* given half the width and half the height of the screen are 400 and 300, so we need to add that
* `love.graphics.translate(-player.x + 400, -player.y + 300)`
	* that is the same as `love.graphics.translate(camera_offset.x, camera_offset.y)` actually

3. let's see the code
```lua
function love.draw()
	love.graphics.translate(-player.x + 400, -player.y + 300)
	-- draw player
	love.graphics.circle("line", player.x, player.y, player.size)
	love.grpahics.draw(player.image, player.x, player.y,
		0, 1, 1, player.image:getWidth()/2, player.image:getHeight()/2)
	-- draw coins
	for i,v in ipairs(coins) do
		love.graphics.circle("line", v.x, v.y, v.size)
		love.graphics.draw(v.image, v.x, v.y,
			0, 1, 1, v.image:getWidth()/2, v.image:getHeight()/2)
	end
end
```
![camera_center_moving.gif (784×590) (sheepolution.com)|560](https://sheepolution.com/images/book/22/camera_center_moving.gif)
___

## HUD
*heads-up display*

now we want to keep score of how many coins we have picked up
It's something that we always want to see on screen
* not part of the play area
* it's part of the HUD

1. firstly we introduce the score mechanics
```lua
function love.load()
	score = 0
	...
	for i=#coins,1,-1 do
		if checkCollision(player, coins[i]) then
			...
			score = score + 1
		end
	end
end

function love.draw()
	love.graphics.translate(-player.x + 400, -player.y + 300)
	-- draw player and draw coins
	...
	-- print score
	love.graphics.print(score, 10, 10)
end
```

simply printing the score like this, it's in the player
* so `love.graphics.translate(-player.x + 400, -player.y + 300)` will move our printed score
* but we don't want that

2. one solution is that, we translate it back to the normal position
```lua
function love.draw()
	love.graphics.translate(-player.x + 400, -player.y + 300)
	...
	-- 1.
	love.graphics.translate(player.x - 400, player.y - 300)
	-- 2.
	love.graphics.origin()
	
	love.graphics.print(score, 10, 10)
end
```

`love.graphics.origin()`
* reset all the coordinate transformations
* since it's below the `love.graphics.translate()` and the code for drawing player and coins
	* it doesn't affect the them but only the score print message

but it's not always the best option
* perhaps we want to scale the game, so we need `love.graphics.scale(2)`
* but we don't want that to apply to our print message

3. For that, there are these `love.graphics.push()` and `love.graphics.pop()` functions
* `love.graphics.push()` saves the current coordinate transformations, and add it to a *stack*
* `love.graphics.pop()` takes the last saved coordinate transformations, and apply it

```lua
function love.draw()
	love.graphics.push() -- here we saved the original coordinate transformations
		love.graphics.translate(-player.x + 400, -player.y + 300)
		-- draw player and coins
		...
	love.graphics.pop() -- here we reset the coordinate transformations
	love.graphics.print(score, 10, 10)
end
```
___

## Screenshake

1. we need a timer, for how long the shake needs to last
```lua
function love.load()
	...
	shakeDuration = 0
	...
end

function love.update(dt)
	if shakeDuration > 0 then
		shakeDuration = shakeDuration - dt
	end
	...
end
```

2. we translate a few pixels in random directions as long as the timer is higher than 0
```lua
function love.draw()
	love.graphics.push()
		love.graphics.translate(-player.x + 400, -player.y + 300)
		
		if shakeDuration > 0 then
			-- it won't reset and override our previous translate()
			love.graphics.translate(love.math.random(-5,5), love.math.random(-5,5))
		end
		...
	love.graphics.pop()
	love.graphics.print(score, 10, 10)
end
```

3. every time we pick up a coin, set the timer
```lua
function love.update()
	...
	-- draw coins
	for i=#coins,1,-1 do
		if checkCollision(player, coins[i]) then
			...
			shakeDuration = 0.3
		end
	end
end
```

![shake.gif (574×253) (sheepolution.com)](https://sheepolution.com/images/book/22/shake.gif)

4. but we forgot to use delta time
* the shake might be too fast for some computers
* we actually need a second timer for that

```lua
function love.load()
	...
	shakeDuration = 0
	shakeWait = 0
	shakeOffset = {x = 0, y = 0}
	...
end

function love.update(dt)
	if shakeDuration > 0 then
		shakeDuration = shakeDuration - dt
		if shakeWait > 0 then
			shakeWait = shakeWait - dt
		else
			shakeOffset.x = love.math.random(-5,5)
			shakeOffset.y = love.math.random(-5,5)
			shakeWait = 0.05
		end
	end
end

function love.draw()
	love.graphics.push()
		love.graphics.translate(-player.x + 400, -player.y + 300)
		if shakeDuration > 0 then
			love.graphics.translate(shakeOffset.x, shakeOffset.y)
		end
		...
	love.graphics.pop()
	love.graphics.print(score, 10, 10)
end
```

now the shake is consistent
___

# Canvas
let's introduce split screen

whenever we use `love.graphics` to draw something, it's drawn on a *canvas*
* it's an object, so we actually can create our own canvas objects

why creating canvas objects?
* to do a split screen, we would draw the game on a separate canvas for each player
* then draw those canvases, to the main canvas
	* wah? how that's possible?
	* canvas is a [Drawable](https://love2d.org/wiki/Drawable)
		* Drawable is a superclass
		* and all LÖVE object that have this superclass can be drawn with `love.graphics.draw()`
	* supertypes of Canvas are
		* [Texture](https://love2d.org/wiki/Texture) < Drawable < [Object](https://love2d.org/wiki/Object)
		* ie. Canvas is inheriting Texture, which inherit Drawable, which inherit Object
* we can create a canvas with `love.graphics.newCanvas()`

> [!tips]
> * both Image and Canvas are a subtype of Texture
> 	* both can be drawn using Quad
> * All LÖVE objects are of the type Object
> 	* although the variable we use for classes is also name `Object`
> 	* they are not related in any way, just named the same

let's start making our split screen
1. first we add another player, and remove the screen shake and save/load system
* since we don't need them in this example

```lua
function love.load()
	player1 = {
		x = 100,
		y = 100,
		size = 25,
		image = love.graphics.newImage("face.png")
	}
	player 2 = {
		x = 300,
		y = 100,
		size = 25,
		image = love.graphics.newImage("face.png")
	}
	
	coins = {}
	for i=1,25 do
		table.insert(coins,
			{
				x = math.random(50, 650),
				y = math.random(50, 450),
				size = 10,
				image = love.graphics.newImage("dollar.png")
			}
		)
	end
	
	score1 = 0
	score2 = 0
end

function love.update(dt)
	-- player 1 controls
	if love.keyboard.isDown("left") then
		player1.x = player1.x - 200 * dt
	elseif love.keyboard.isDown("right") then
		player1.x = player1.x + 200 * dt
	end
	
	if love.keyboard.isDown("up") then
		player1.y = player1.y - 200 * dt
	elseif love.keyboard.isDown("down") then
		player1.y = player1.y + 200 * dt
	end
	
	-- player 2 controls
	if love.keyboard.isDown("a") then
		player2.x = player2.x - 200 * dt
	elseif love.keyboard.isDown("d") then
		player2.x = player2.x + 200 * dt
	end
	
	if love.keyboard.isDown("w") then
		player2.y = player2.y - 200 * dt
	elseif love.keyboard.isDown("s") then
		player2.y = player2.y + 200 * dt
	end
	
	for i=#coins,1,-1 do
		if checkCollision(player1, coins[i]) then
			table.remove(coins, i)
			player1.size = player1.size + 1
			score1 = score1 + 1
		elseif checkCollision(player2, coins[i]) then
			table.remove(coins, i)
			player2.size = player2.size + 1
			score2 = score2 + 1
		end
	end
end

function love.draw()
	love.graphics.push()
		love.graphics.translate(-player1.x + 400, -player1.y + 300)
		
		love.graphics.circle("line", player1.x, player1.y, player1.size)
		love.graphics.draw(player1.image, player1.x, player1.y,
			0, 1, 1, player1.image:getWidth()/2, player1.image:getHeight()/2)
		
		love.graphics.circle("line", player2.x, player2.y, player2.size)
		love.graphics.draw(player2.image, player2.x, player2.y,
			0, 1, 1, player2.image:getWidth()/2, player2.image:getHeight()/2)
		
		for i,v in ipairs(coins) do
			love.graphics.circle("line", v.x, v.y, v.size)
			love.graphics.draw(v.image, v.x, v.y,
				0, 1, 1, v.image:getWidth()/2, v.image:getHeight()/2)
		end
	love.graphics.pop()
	love.graphics.print("Player 1 - " .. score1, 10, 10)
	love.graphics.print("Player 2 - " .. score2, 10, 30)
end

function checkCollision(p1, p2)
	local distance = math.sqrt((p1.x - p2.x)^2 + (p1.y - p2.y)^2)
	return distance < p1.size + p2.size
end
```

2. now we need a canvas to draw on
* how big the canvas should be?
* well, we eventually will have 2 screens, splitted left and right, so each canvas should be 400 x 600

* we create a canvas by
```lua
function love.load()
	screenCanvas = love.graphics.newCanvas(400, 600)
	...
end
```

* next we need to draw the game on to the new canvas
	* we use `love.graphics.setCanvas()` to set the canvas we want to draw on
	* if we pass no argument to this function, it will reset to the default canvas
```lua
function love.draw()
	love.graphics.setCanvas(screenCanvas)
	... -- our drawings
end
```

but if we run the game now, we'll see nothing
because the game is drawn on to our new canvas, but nothing is drawn onto the default canvas
* we need to draw our canvas onto it
	* since Canvas is a Drawable, and Drawable can be drawn with `love.graphics.draw()`
```lua
function love.draw()
	love.graphics.setCanvas(screenCanvas)
	... -- draw player and coins
	love.graphics.setCanvas()
	
	-- draw the canvas
	love.graphics.draw(screenCanvas)
	
	-- draw scores
	love.graphics.print("Player 1 - " .. score1, 10, 10)
	love.graphics.print("Player 2 - " .. score2, 10, 10)
end
```

3. now we can see our game, but some weird thing is happening
* when we move, we're leaving a trail behind
* it's because we didn't clear the canvas
	* meaning everything that we drew, remains on the canvas
	* this is a default behavior

we need to call `love.graphics.clear()` to clear the canvas
* put this right before drawing the game

also now we need to draw the game twice
* since we have 2 canvases
* and let's put everything we draw for our game in a fucntion
	* so that we don't need to repeat the code

```lua
function love.draw()
	-- left canvas
	love.graphics.setCanvas(screenCavas)
		love.graphics.clear()
		drawGame()
	love.graphics.setCanvas()
	love.graphics.draw(screenCanvas)
	
	-- right canavas
	love.graphics.setCanvas(screenCanvas)
		love.graphics.clear()
		drawGame()
	love.graphics.setCanvas()
	love.graphics.draw(screenCanvas, 400)
	
	-- add a line to separate the screens
	love.graphics.line(400, 0, 400, 600)
	
	-- draw scores
	love.graphics.print("Player 1 - " .. score1, 10, 10)
	love.graphics.print("Player 2 - " .. score2, 10, 30)
end

function drawGame()
	love.graphics.push()
		love.graphics.translate(-player1.x + 200, -player1.y + 300)
		
		love.graphics.circle("line", player1.x, player1.y, player1.size)
		love.grpahics.draw(player1.image, player1.x, player1.y,
			0, 1, 1, player1.image:getWidth()/2, player1.image:getHeight()/2)
		
		love.graphics.circle("line", player2.x, player2.y, player2.size)
		love.graphics.draw(player2,.image, player2.x, player2.y,
			0, 1, 1, player2.image:getWidth()/2, player2.image:getHeight()/2)
		
		for i,v in ipairs(coins) do
			love.graphics.circle("line", v.x, v.y, v.size)
			love.graphics.draw(v.image, v.x, v.y,
				0, 1, 1, v.image:getWidth()/2, v.image:getHeight()/2)
		end
	love.graphics.pop()
end
```

4. now we have a split screen, but both the left and right screens focus on player 1
* we should make the camera focus on a different player in a different screen
* we'll add a `focus` parameter to `drawGame()`, and it would be either `player1` or `player2`

```lua
function love.draw()
	love.graphics.setCanvas(screenCanvas)
		love.graphics.clear()
		drawGame(player1)
	love.graphics.setCanvas()
	love.graphics.draw(screenCanvas)
	
	love.graphics.setCanvas(screenCanvas)
		love.graphics.clear()
		drawGame(player2)
	love.graphics.setCanvas()
	love.graphics.draw(screenCanvas)
	
	...
end

function drawGame(focus)
	love.graphics.push()
		love.graphics.translate(-focus.x + 200, -focus.y + 300)
		...
	love.graphics.pop()
end
```

![splitscreen.gif (786×576) (sheepolution.com)](https://sheepolution.com/images/book/22/splitscreen.gif)
___

## Camrea Libraries

we made a rather simple camera
* but maybe we'd want more functionality to it
* eg. zooming in, borders, etc.

check out some cool libraries for that
* [kikito/gamera: A camera system for LÖVE (github.com)](https://github.com/kikito/gamera)
* [a327ex/STALKER-X: Camera module for LÖVE (github.com)](https://github.com/a327ex/STALKER-X)
___
