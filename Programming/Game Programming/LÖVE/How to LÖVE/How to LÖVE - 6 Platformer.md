# Resolving collision

previously we talked about [[How to LÖVE - 3#Detect Collision|detecting collisions]]
* in that chapter, we just make something deleted upon collision

but imagine we have a game where player can move around, and there are walls that player can't walk through, in this case just detecting collisions isn't all the functionalities we need
* we also need to stop player when it touches the wall
* and we don't want the player to get stuck inside the wall, we need to push it back

> [!quote]
> Before we start, I want to tell you that resolving collision is very hard, and it's something that even professionals have trouble dealing with. Look at speedruns for example. There are a lot of glitches where you are able to clip through walls and such. The collision we're going to make is pretty solid but far from perfect.

___

## prepare a game

for this tutorial we need 2 things
* a player, that can walk in 4 directions ![player.png (50×100) (sheepolution.com)|30](https://sheepolution.com/images/book/23/player.png)
* walls ![wall.png (50×50) (sheepolution.com)|40](https://sheepolution.com/images/book/23/wall.png)

notice we put the colorful borders in the images
it helps us to see if there is collision

let's make a base class for both of these, an `Entity` class
* therefore the game project will have the following files:
	* `main.lua`, `classic.lua`, `entity.lua`, `player.lua`, `wall.lua`

* the `Entity` class needs an `x`, `y`, `width`, `height`, and an image
* also a function for checking collisions
```lua
--! file: entity.lua
Entity = Object:extend()

function Entity:new(x, y, image_path)
	self.x = x
	self.y = y
	self.image = love.graphics.newImage(image_path)
	self.width = self.image:getWidth()
	self.height = self.image:getHeight()
end

function Entity:update(dt)
	-- empty for now
end

function Entity:draw()
	love.graphics.draw(self.image, self.x, self.y)
end

function Entity:checkCollision(e)
	-- e will be the other entity
	return self.x + self.width > e.x
	and self.x < e.x + e.width
	and self.y + self.height > e.y
	and self.y < e.y + e.height
end
```

now we make the subclasses, `Player` and `Wall`

```lua
--! file: wall.lua
Wall = Entity:extend()

function Wall:new(x, y)
	Wall.super.new(self, x, y, "wall.png")
end
```

```lua
--! file: player.lua
Player = Entity:extend()

function Player:new(x, y)
	Player.super.new(self, x, y, "player.png")
end

function Player:update(dt)
	if love.keyboard.isDown("left") then
		self.x = self.x - 200 * dt
	elseif love.keyboard.isDown("right") then
		self.x = self.x + 200 * dt
	end
	
	if lov.ekeyboard.isDown("up") then
		self.y = self.y - 200 * dt
	elseif love.keyboard.isDown("down") then
		self.y = self.y + 200 * dt
	end
end
```

then we can create these objects in `main.lua`
```lua
--! file: main.lua
function love.load()
	Object = require "classic"
	require "entity"
	require "player"
	require "wall"
	
	player = Player(100, 100)
	wall = Wall(200, 100)
end

function love.update(dt)
	player:update(dt)
	wall:update(dt)
end

function love.draw()
	player:draw()
	wall:draw()
end
```

now we can walk around
but surely we can walk through our wall
___

## attempt 1 - last position

maybe we can keep track of our previous position
then whenever we hit a wall, we just spawn back to that position
* so the player can never go through a wall

for this, we need to add something to our `Entity` class
* a `last` object, to keep track of the previous positions
* beside the function for checking collisions, a function for put it back to the previous position
```lua
--! file: entity.lua
Entity = Object:extend()

function Entity:new(x, y, image_path)
	...
	self.last = {}
	self.last.x = self.x
	self.last.y = self.y
end

function Entity:update(dt) -- passing dt here because Player:update() used dt
	self.last.x = self.x
	self.last.y = self.y
end

function Entity:resolveCollision(e)
	if self:checkCollision(e) then
		self.x = self.last.x
		self.y = self.last.y
	end
end
```

since we added `Entity:update(dt)`
`Player:update(dt)` need to call it
```lua
--! file: player.lua
function Player:update(dt)
	Player.super.update(self, dt)
	...
end
```

In `main.lua`, we need to call the player's `resolveCollision()` function
```lua
--! file: main.lua
function love.update(dt)
	player:update(dt)
	wall:update(dt)
	player:resolveCollision(wall)
end
```

it works... sort of.
if we look close enough, there is sometimes a small gap between the player and the wall
* because the player can't really touch the wall
* since we're pushing the player back to its previous location
![collision_to_previous.gif (387×163) (sheepolution.com)](https://sheepolution.com/images/book/23/collision_to_previous.gif)
___

## attempt 2 - wall's edge

instead of placing the player on its previous position,
we should move the player until it doesn't touch the wall anymore
* we push it back just enough so that it doesn't overlap with the wall
	* so that the edges are touching

then it raises 2 questions for us to solve
* how far should we move the player?
* in which direction should we push the player away?

assuming the player comes from the left,
if the player's right side is on x-position 225, and the wall's left side is on x-position 205
* then we put the player back to the left by `225 - 205 = 20` pixels

to figure out from which direction the player is coming from, we can use our previous position

* we can try subtracting the player's position by the previous position, to know in which direction the player is moving
	* but it's not the solution because the player can move in 8 directions instead of just left, right, up, down
* if the player moves towards top-right, this way we would just know it's coming from bottom-left, and we would push it back to both left and down
	* this is not the behavior we want
	* because the player may not come from bottom-left, but just left or bottom

the solution is that, if the player collides with the box,
it's previous position was already vertically or horizontally aligned with the wall
* unless it came from the corner

* if it came from the left, the player and the wall are already vertically aligned
* they are already colliding, on a vertical level
	* player's bottom side is further to the bottom than the wall's top side
	* player's top side is further to the top than the wall's bottom side
![hor_ver_collision.gif (504×251) (sheepolution.com)](https://sheepolution.com/images/book/23/hor_ver_collision.gif)

Then, to collide with the wall, the player must move horizontally
* we just don't know whether it's from the left or the right
* let's add the code for checking whether it was already vertically or horizontally aligned first

```lua
--! file: entity.lua
...
function Entity:wasVerticallyAligned(e)
	return self.last.y < e.last.y + e.height and self.last.y + self.height > e.last.y
end

function Entity:wasHorizontallyAligned(e)
	return self.last.x < e.last.x + e.width and self.last.x + self.width > e.last.x
end

function Entity:resolveCollision(e)
	if self:checkCollision(e) then
		if self:wasVerticallyaligned(e) then
		
		elseif self:wasHorizontallyAligned(e) then
		
		end
	end
end
```

now we need to check from which side they're touching
* a simple way is to check the center of both the wall and the player
	* eg. if the player was already vertically aligned, and its center is more to the left than the center of the wall, we push it to the left

> [!info]
> I believe by checking in which direction the player or the wall is moving could make a better solution. But here we'll just stick with the tutorial.

```lua
--! file: entity.lua
function Entity:resolveCollision(e)
	if self:checkCollision(e) then
		if self:wasVerticallyAligned(e) then
			if self.x + self.width/2 < e.x + e.width/2 then
				-- if the self's center is to the left of e's center
				local pushback = self.x + self.width - e.x
				self.x = self.x - pushback
			else
				local pushback = e.x + e.width - self.x
				self.x = self.x + pushback
			end
		elseif self:wasHorizontallyAligned(e) then
			if self.y + self.height/2 < e.y + e.height/2 then
				-- if the self's center is to the top of e's center
				local pushback = self.y + self.height - e.y
				self.y = self.y - pushback
			else
				local pushback = e.y + e.height - self.y
				self.y = self.y + pushback
			end
		end
	end
end
```

![loss.png (327×319) (sheepolution.com)](https://sheepolution.com/images/book/23/loss.png)
let's recap
1. the player and the wall are already vertically aligned
	* so the player must moved horizontally to collide with the wall
2. by check if the center of player is to the left or to the right of the center of the wall
	* we know in which direction the player came
	* and thus in which direction we need to push the player back
3. we calculate by how much we need to push back the player
4. the player is pushed back correctly!
	* the player and the wall are just touching, but not overlapping
___

## strength

okay, so that works
but there is this potential problem
* right now we call `player:resolveCollsion(wall)` for that
* what if we were to call it the other way around, `wall:resolveCollision(player)`
	* well, it's going to push back the wall instead of the player
* surely in this instance we know that the wall is stronger than the player
	* but let's assume we don't know that

to fix that, we can add a variable, `strength`
* the entity with a lower `strength` gets pushed away upon collisions

```lua
--! file: entity.lua
Entity = Object:extend()

function Entity:new(x, y, image_path)
	...
	self.strength = 0 -- default value
end
```

```lua
--! file: wall.lua
Wall = Entity:extend()

function Wall:new(x, y)
	Wall.super.new(self, x, y, "wall.png")
	self.strength = 100 -- give the wall a strength of 100
end
```

now in `Entity:resolveCollision(e)`
* if the value of `self.strength` is higher than the value of `e.strength`
* we call `e:resolveCollision(self)`, to turn the roles around

```lua
--! file: entity.lua
function Entity:resolveCollision(e)
	if self.strength > e.strength then
		e:resolveCollision(self)
		return -- return because we don't want to continue the function
	end
	
	if self:checkCollision(e) then
		...
	end
end
```

now it doesn't matter in which order we call the function
___

# Push a box
so now the wall pushes the player away,
what to do if we want to add a box that player can push away?

1. let's create a `Box` class
* a box would look like ![box.png (50×50) (sheepolution.com)|30](https://sheepolution.com/images/book/23/box.png)
```lua
--! file: box.lua
Box = Entity:extend()

function Box:new(x, y)
	Box.super.new(self, x, y, "box.png")
end
```

2. then we want to add the box in game
* also, `Entity` objects in game are getting many now, so let's make a table for them
```lua
--! file: main.lua
function love.load()
	Object = require "classic"
	require "entity"
	require "player"
	require "wall"
	require "box"
	
	player = Player(100, 100)
	wall = Wall(200, 100)
	box = Box(400, 150)
	
	objects = {}
	table.insert(objects, player)
	table.insert(objects, wall)
	table.insert(objects, box)
end
```

3. now we can loop through the `objects` to update and draw them
* what about resolving the collision?
	* we want all the objects to check collision with all other objects
* to achieve this we could simply use 2 for-loops, like so
```lua
for i,v in ipairs(objects) do
	for j,w in ipairs(objects) do
		if v ~=w then
			v:resolveCollision(w)
		end
	end
end
```

But that is quite redundant, because we're check collision twice for each object
* since if we called `player:resolveCollision(wall)`, we don't need to call `wall:resolveCollision(player)`
* so in the first loop we'd need to go through the list of objects
	* but in the second loop we'd start from the next object of the object that the first one is checking
```lua
--! file: main.lua
...
function love.update(dt)
	-- update all objects
	for i,v in ipairs(objects) do
		v:update(dt)
	end
	
	-- go through all the objects, except the last
	for i=1,#objects-1 do
		-- go through all the objects, starting from the position i + 1
		for j=i+1,#objects do
			objects[i]:resolveCollision(objects[j])
		end
	end
end

function love.draw()
	-- draw all objects
	for i,v in ipairs(objects) do
		v:draw()
	end
end
```

![collision_checks.png (555×355) (sheepolution.com)](https://sheepolution.com/images/book/23/collision_checks.png)
here is a visualization of what the 2 for-loops are doing
* the upper row is the first for-loop
* the bottom row is the second for-loop
* the lines are indicating which 2 objects are checking collision in an iteration

4. make the box pushable
* we need to make the player stronger than the box
```lua
--! file: player.lua
Player = Entity:extend()

function Player:new(x, y)
	Player.super.new(self, x, y, "player.png")
	self.strength = 10
end
...
```

okay we can push the box now
but let's try pushing the box against the wall
![box_wall_bad.gif (387×264) (sheepolution.com)](https://sheepolution.com/images/book/23/box_wall_bad.gif)

why it happens?
* the player pushes the box, but the wall pushes back the box
* we expect the player to stop in this case
	* but since the player is stronger than the box, it can still continue pushing
	* that is the same as, moving

the solution is obvious, we need to pass the strength of the wall onto the box
* so the box is strong enough to push back the player away
	* when it is pushed against the wall
* but this strength should only be temporary

* let's create a property called `tempStrength`
	* *temp* is short for *temporary*
```lua
--! file: entity.lua
Entity = Object:extned()

function Entity:new(x, y, image_path)
	...
	self.strength = 0
	self.tempStrength = 0
end

function Entity:update(dt)
	self.last.x = self.x
	self.last.y = self.y
	-- reset tempStrength
	self.tempStrength = self.strength
end

function Entity:resolveCollision(e)
	-- compare the tempStrength, instead of strength
	if self.tempStrength > e.tempStrength then
		e:resolveCollision(self)
		return
	end
	
	if self:checkCollision(e) then
		-- copy the tempStrength
		self.tempStrength = e.tempStrength
		...
	end
return
```

5. yeah now the above problem is solved, but another arises
* the player pushes box into the wall, then the wall pushes the box back into the player
* but then, nothing is pushing back the player
	* because the we checked the collision between the box and the player already
	* the for-loop for checking collisions has been run

the solution is, we need to run the checking again
* run the for-loop again
* in fact, whenever a collision occur, we need to run the checking once more

we implement this by making the function `Entity:resolveCollision(e)` returns `true` or `false`
* so if it returns `true`, we would go through the objects and checking again

```lua
--! file: entity.lua
function Entity:resolveCollision(e)
	if self.tempStrength > e.tempStrength then
		return e:resolveCollision(self)
	end
	
	if self:checkCollision(e) then
		...
		-- there was collision, after resolved collision, return true
		return true
	end
	
	-- there was no collision
	-- 1.
	return false
	-- 2. return nothing, so the returned value will be nil
	
end
```

now we want the checking runs as long as the function returned `true`
* it seems like a job for while-loop

> [!warning]
> Add a limit to the number of loops for while-loop. So that if the condition is somehow always true, we won't end up in an infinite loop and manage to escape eventually.

```lua
--! file: main.lua
function love.update(dt)
	for i,v in ipairs(objects) do
		v:update(dt)
	end
	
	local loop = true
	local limit = 0
	
	while loop do
		-- if no collision happened, the loop keep being false and exit the loop
		loop = false
		
		limit = limit + 1
		if limit > 100 then
			break -- we probably stuck in an endless loop, so break it
		end
		
		for i=1,#objects-1 do
			for j=i+1,#objects do
				local collision = objects[i]:resolveCollision(objects[i])
				if collision then
					loop = true
				end
			end
		end
	end
end
```

![box_wall_good.gif (408×292) (sheepolution.com)](https://sheepolution.com/images/book/23/box_wall_good.gif)
___

# Collision Libraries

for more advanced collision handling,
check out [kikito/bump.lua](https://github.com/kikito/bump.lua)

for collision with shapes other than rectangles,
check out [vrld/HC](https://github.com/vrld/HC)
___

# Platformer
since we have a collision that works quite okay, we can make a platformer
* player can jump up
* then fall down back to ground

1. firstly let's create a map to walk around in
```lua
function love.load()
	Object = require "classic"
	require "entity"
	require "player"
	require "wall"
	require "box"
	
	player = Player(100, 100)
	box = Box(400, 150)
	
	objects = {}
	table.insert(objects, player)
	table.insert(objects, box)
	
	map {
		{1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1},
		{1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
		{1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
		{1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
		{1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
		{1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1},
		{1,0,0,0,0,0,0,0,0,0,0,0,0,1,1,1},
		{1,0,0,0,0,0,0,0,0,0,0,0,0,1,1,1},
		{1,0,0,0,0,0,0,0,0,0,0,0,0,1,1,1},
		{1,0,0,0,0,0,1,1,1,1,1,1,1,1,1,1},
		{1,0,0,0,0,0,1,1,1,1,1,1,1,1,1,1},
		{1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1}
	}
	
	for i,v in ipairs(map) do
		for j,w in ipairs(v) do
			if w == 1 then
				table.insert(objects, Wall((j-1)*50, (i-1)*50))
			end
		end
	end
end
```
![map.png (800×600) (sheepolution.com)](https://sheepolution.com/images/book/24/map.png)


## skip collision checks

2. depending on how good your computer is, the game can become very slow
* because all the walls we added are checking collision with each other
* but we don't really need them to check for collisions
	* they never move, so they will never overlap with each other

therefore, we should have a separate table for all the walls
* the `objects` table will check collision with the `walls` table
* but the `walls` table does not check for collision

```lua
function love.load()
	...
	walls = {}
	map = { ... }
	for i,v in ipairs(map) do
		for j,w in ipairs(v) do
			if w == 1 then
				table.insert(walls, Wall((j-1)*50, (i-1)*50))
			end
		end
	end
end

function love.update()
	for i,v in ipairs(objects) do
		v:update(dt)
	end
	for i,v in ipairs(walls) do
		v:update(dt)
	end
	
	local loop = true
	local limit = 0
	
	while loop do
		...
		-- for each object check collision with every wall
		for i,wall in ipairs(walls) do
			for j,object in ipairs(objects) do
				local collision = object:resolveCollision(wall)
				if collision then
					loop = true
				end
			end
		end
	end
end

function love.draw()
	for i,v in ipairs(objects) do
		v:draw()
	end
	for i,v in ipairs(walls) do
		v:draw()
	end
end
```
___

## Falling
so we want to make the player falls down. easy.

```lua
--! file: player.lua
function Player:update(dt)
	Player.super.update(self, dt)
	... -- input stuffs
	self.y = self.y + 200 * dt -- auto fall down
end
```
![falling.gif (789×588) (sheepolution.com)](https://sheepolution.com/images/book/24/falling.gif)

that... looks quite odd, isn't it?
* it's not how gravity works
* falling objects have an acceleration. as it falls, it should gain speed.

let's get something more like real gravity in the `Entity` class
* a `gravity` and a `weight` property
	* `gravity` to increase the y-position of the entity
	* `weight` to increase the gravity

```lua
--! file: entity.lua
function Entity:new(x, y, image_path)
	...
	self.gravity = 0
	self.weight = 400
end

function Entity:update(dt)
	...
	self.gravity = self.gravity + self.weight * dt
	self.y = self.y + self.gravity * dt
end
```

since the walls don't need to fall, we give it a `weight` of `0`
```lua
--! file: wall.lua
Wall = Entity:extend()

function Wall:new(x, y)
	Wall.super.new(self, x, y, "wall.png", 1)
	self.strength = 100
	self.weight = 0
end
```

and remember to remove the part of the player that makes it automatically fall
* as well as moving up by pressing the up-key

![gravity.gif (789×588) (sheepolution.com)](https://sheepolution.com/images/book/24/gravity.gif)

looks better
but if we run the game long enough, they can fall right through the floor

it's because the gravity keeps increasing even though they are standing on the floor
* so we need to reset the gravity for them
* we can do this in `Entity:resolveCollision(e)`
	* when they touch the floor

```lua
--! file: entity.lua
function Entity:resolveCollision(e)
	...
	if self:checkCollision(e) then
		self.tempStrength = e.tempStrength
		if self:wasVerticallyAligned(e) then
			...
		elseif self:wasHorizontallyAligned(e) then
			if self.y + self.height/2 < e.y + e.height/2 then
				...
				-- we're touching a wall from the bottom
				-- meaning we're standing on something
				self.gravity = 0
			else
				...
			end
		end
		return true
	end
	return false
end
```
___

## Jumping

1. we make it jump when the up-key is pressed
* call the `jump()` function

```lua
--! file: main.lua
function love.keypressed(key)
	if key == "up" then
		player:jump()
	end
end
```

so what happen in the `jump()` function?
* it's actually very easy. we give the `gravity` a negative value
	* the lower the value, the higher the player jumps
```lua
--! file: player.lua
function Player:jump()
	self.gravity = -300
end
```

![jumping.gif (789×588) (sheepolution.com)](https://sheepolution.com/images/book/24/jumping.gif)

2. but this way it can jump multiple times
    it should be able to jump when we stand on the ground
* we will add a `canJump` property to the player
	* when it lands on the ground, it becomes `true`
	* when it's `true`, player is allowed to `jump()`, and then it'd turn `false`
		* until the next time it lands on a floor

```lua
--! file: player.lua
function Player:new(x, y)
	...
	self.canJump = false
end

function Player.update(dt)
	...
end

function Player:jump()
	if self.canJump then
		self.gravity = -300
		self.canJump = false
	end
end
```

but right now if we want to do an action as we land, we can only do it in the `Entity` class
so we need a function, that is called upon a `Entity` object landed
* so that the `Player` class can override that function
* actually let's make the function get called upon collision, or resolving collision, instead

```lua
--! file: entity.lua
function Entity:resolveCollision(e)
	...
	if self:checkCollision(e) then
		self.tempStrength = e.tempStrength
		if self:wasVerticallyAligned(e) then
			if self.x + self.width/2 < e.x + e.width/2 then
				self:collide(e, "right")
			else
				self:collide(e, "left")
			end
		elseif self:wasHorizontallyAligned(e) then
			if self.y + self.height/2 < e.y + e.height/2 then
				self:collide(e, "bottom")
			else
				self:collide(e, "top")
			end
		end
		return true
	end
	return false
end

function Entity:collide(e, direction)
	if direction == "right" then
		local pushback = self.x + self.width - e.x
		self.x = self.x - pushback
	elseif direction == "left" then
		local pushback = e.x + e.width - self.x
		self.x = self.x + pushback
	elseif direction == "bottom" then
		local pushback = self.y + self.height - e.y
		self.y = self.y - pushback
		self.gravity = 0
	elseif direction == "top" then
		local pushback = e.y + e.height - self.y
		self.y = self.y + pushback
	end
end
```

and now we override the function `collide(e)` in the `Player` class, with the needed functionality
```lua
--! file: player.lua
function Player:collide(e, direction)
	Player.super.collide(self, e, direction)
	if direction == "bottom" then
		self.canJump = true
	end
end
```

![midair.gif (789×588) (sheepolution.com)](https://sheepolution.com/images/book/24/midair.gif)

3. yeah it can only jump once
    but if we walk off a platform and jump, it can still jump mid-air
* although this might be a desired feature, in this example we don't need it
* we can fix this by checking if the previous y-position is not equal to the current y-position
	* because if the y-position is moving, then we're not standing on the ground

```lua
--! file: player.lua
function Player:update(dt)
	...
	if self.last.y ~= self.y then
		self.canJump = false
	end
end
```
___

## Jump-through platform

> [!tips]
> It's very common in platformers that we need to hit something from a certain direction.

eg. Mario
1. we must hit the question mark box from the bottom to get something out of it
2. we can only kill an enemy by jumping on top of it
3. a more advanced example maybe, some platforms we can jump through them from the bottom, but if we hitted it from the top, we just stand on top of it

![platform.png (510×476) (sheepolution.com)|380](https://sheepolution.com/images/book/24/platform.png)

in our example we gonna use the box to mimic these platforms
* so the player doesn't push the box, but instead walks right through it
* when the player jumps on the box it stands on top of it

first the function `collide(e, direction)` is for resolving collisions
* but in this case we don't want the collision to be resolved
* so that we can walk through the box

So how about we first check if both parties of the collision want the collision to be resolved
* we create a function called `checkResolved()`
	* if both `self` and `e` return `true`, only then we continue resolving the collision

```lua
--! file:entity.lua
function Entity:resolveCollision(e)
	...
	if self:checkCollision(e) then
		self. tempStrength = e.tempStrength
		if self:wasVerticallyAligned(e) then
			if self.x + self.width/2 < e.x + e.width/2 then
				local a = self:checkResolve(e, "right")
				local b = e:checkResolve(self, "left")
				if a and b then
					self:collide(e, "right")
				end
			else
				local a = self:checkResolve(e, "left")
				local b = e:checkResolve(self, "right")
				if a and b then
					self:collide(e, "left")
				end
			end
		elseif self:wasHorizontallyAligned(e) then
			if self.y + self.height/2 < e.y + e.height/2 then
				local a = self:checkResolve(e, "bottom")
				local b = e:checkResolve(self, "top")
				if a and b then
					self:collide(e, "bottom")
				end
			else
				local a = self:checkResolve(e, "top")
				local b = self:checkResolve(self, "bottom")
				if a and b then
					self:collide(e, "top")
				end
			end
		end
		return true
	end
	return false
end

function Entity:checkResolve(e, direction)
	return true
end
```

now in `player.lua` we can override the function `checkResolve(e, direction)`

* with the library classic, every class has the function `is:(class)`
	* which can check if an instance of a class is of a certain type of class
	* so we can use `e:is(Box)` to check if `e` is of the type `Box`
* this also works with base classes
	* if `e` were a `Box`, then `e:is(Entity)` would return `true` too
	* since `Box` is an extension of the base class `Entity`

so in `Player:checkResolve(e, direction)`
* we first check if we're colliding with a `Box`
	* then we check if the `direction` is `"bottom"`
	* if both are `true`, return `true`
* if we're colliding with something else, we want to return `true` as well
* `true` meaning we do want the collision to be resolved

```lua
--! file: player.lua
function Player:checkResolve(e, direction)
	if e:is(Box) then
		if direction == "bottom" then
			return true
		else
			return false
		end
	end
	return true
end
```

![through.gif (789×588) (sheepolution.com)](https://sheepolution.com/images/book/24/through.gif)
___
