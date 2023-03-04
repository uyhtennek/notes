# Hitboxes / Hurtboxes
Rectangles is how we do hitbox and hurtbox in our case

Usually we can see from games that the boxes are overlapping
* maybe like a green box encapsulating the whole character, representing the *hurtbox*
* a red box encapsulating the sword or the fist of the character, representing the *hitbox*

* great to look at some fighting games
* Minecraft also has a built-in commands to show the hurtboxes
	* items like a sword on the ground, may not be meant to be hurted, but picked up though

In our game, what the hitbox is for?
* the character's melee attack
* then why don't we just use the character's x, y, width, height?
	* the attack has a direction, and probably not the same size as the character
* also the character's x, y, width, height is essentially his hurtbox

So 2 boxes
1. Hitbox for the player to hit things
2. Hurtbox so other things can hurt player

```lua
--! file: Hitbox.lua
Hitbox = Class{}

function Hitbox:init(x, y, width, height)
	self.x = x
	self.y = y
	self.width = width
	self.height = height
end
```

It's just a rectangle class
* That's it! we don't need more than that.
* since our `collide()` just expect a rectangle

```lua
--! file: PlayerSwingSwordState.lua
-- when player hit space bar, it triggers this swing sword state
...
function PlayerSwingSwordState:init(player, dungeon)
	...
	-- for creating hitbox and choosing the animation as well
	local direction = self.player.direction
	local hitboxX, hitboxY, hitboxWidth, hitboxHeight
	
	if direction == 'left' then
		hitboxWidth = 8
		hitboxHeight = 16
		hitboxX = self.player.x - hitboxWidth
		hitboxY = self.player.y + 2
	elseif direction == 'right' then
		...
	elseif direction == 'up' then
		...
	else
		...
	end
	
	self.swordHitbox = Hitbox(hitboxX, hitboxY, hitboxWidth, hitboxHeight)
	self.player:changeAnimation('sword-' .. self.player.direction)
end

function PlayerSwingSwordState:enter(params)
	gSounds['sword']:stop()
	gSounds['sword']:play()
	
	-- restart animation
	self.player.currentAnimation:refresh()
end

function PlayerSwingSwordState:update(dt)
	for k, entity in pairs(self.dungeon.currentRoom.entities) do
		-- loop over the entities in the room
		if entity:collides(self.swordHitbox) then
			-- if it collides with the hitbox
			entity:damage(1) -- and if the entity's hp goes down to 0, it's dead
			gSounds['hit-enemy']:play()
		end
	end
	
	if self.player.currentAnimation.timesPlayed > 0 then
		-- if the animation gets to the end, back to the idle state
		self.player.currentAnimation.timesPlayed = 0
		self.player.changeState('idle')
	end
	
	-- we can keep swinging too
	if love.keyboard.wasPressed('space') then
		-- restart the logic again (generate the hitbox and play the animation)
		self.player:changeState('swing-sword')
	end
end

function PlayerSwingSwordState:render()
	local anim = self.player.currentAnimation
	love.graphics.draw(gTextures[anim.texture], ...)
	
	-- better to draw the boxes out for debug sake
	love.graphics.setColor(255, 0, 255, 255)
	love.graphics.rectangle('line', ...) -- draw hurtbox
	love.graphics.rectangle('line', ...) -- draw hitbox
	love.graphics.setColor(255, 255, 255, 255)
end
```

Also we can just easily spawn more hitboxes for other entities and objects
* or maybe we'd have projectiles that are offensive
	* just assign a hitbox to the projectile or just use its x, y, width, height
___

# Events

It's essentially just,
"When something happens, do this block of code"

* and we can do it anywhere
* maybe like we want an object do something when another object did some certain thing
	* but we don't want to pass references back and forth
* eg. when ever the player swing his sword, look at all the entities and see whether or not the hitbox collides with them
	* it did the same thing as before
	* but we can place the logic in some other centralized location if we want to


## eg. Achievement

a game can have maybe a 1,000 achievements, do we want to put all the checks scattered in all the update functions in all places?
* instead we can broadcast an event for all the interactions
	* whether or not the achievement is met
* eg. event on picked up coin, event on killed creature, event on player jumps, increment some counter, stored somewhere else
	* all the if checks are in somewhere else
	* instead of the code where our main game or game loop is happening


## `knife.event`

`Event.on(name, callback)`
* so on something, we want to do `callback`
* and the event is registered as `name`
* then do the broadcast in some code, via `Event.dispatch()`
	* *dispatch* = 派遣

`Event.dispatch(name, [params])`
* broadcast the `name` event, meaning run the callback function registered in `Event.on()`
* eg. if player jumps, do `Event.dispatch()`, and pass in whatever values our callback function needs, maybe like the x, y position where the player jumps to

As a result, the checks or the logic that might be rather long,
just replaced by a single `Event.dispatch()` in the player's update

let's say we want to give the player an achievement when he jumps off a cliff
* the jumping logic doesn't care where the player jumps to, or whether there is a cliff there
* so that question, that check, should be delegated to somewhere else

```lua
--! file: PlayerWalkState.lua
function PlayerWalkState:update(dt)
	-- check for inputs
	...
	EntityWalkState.update(self, dt) -- so every entity can do this code
	-- which just perform base collision detection against walls
	-- it raise a bumped flag if it does collide with walls
	
	if self.bumped then
		-- if the player bumped a wall
		if self.entity.direction == 'left' then
			-- since when entities bumped a wall, it knocks the entity back
			-- here we're setting the player's position into the wall again
			self.entity.x = self.entity.x - PLAYER_WALK_SPEED * dt
			
			for k, doorway in pairs(self.dungeon.currentRoom.doorways) do
				if self.entity:collides(doorway) and doorway.open then
					-- shift the player to the center of the doorway
					self.entity.y = doorway.y + 4
					-- call an event
					Event.dispatch('shift-left')
				end
			end
			
			-- set the player out of the wall again
			self.entity.x = self.entity.x + PLAYER_WALK_SPEED * dt
		end
		elseif self.entity.direction == 'right' then
			...
					Event.dispatch('shift-right')
		elseif self.entity.direction == 'up' then
			...
					Event.dispatch('shift-up')
		else -- down
			...
					Event.dispatch('shift-down')
		end
	end
end
```

So make a guess what `Event.on('shift-')` does?
* move to the next room
* spawn the next room
* shift the 'camera'

```lua
--! file: Dungeon.lua
function Dungeon:init(player)
	...
	Event.on('shift-left', function()
		self:beginShifting(-VIRTUAL_WIDTH, 0)
	end)
	
	Event.on('shift-right', function()
		self.beginShifting(VIRTUAL_WIDTH, 0)
	end)
	
	Event.on('shift-up', function()
		self.beginShifting(0, -VIRTUAL_HEIGHT)
	end)
	
	Event.on('shift-down', function()
		self.beginShifting(0, VIRTUAL_HEIGHT)
	end)
end
-- why we need to pass in the x, y?
-- recall the rooms have `adjacentOffsetX` and `adjacentOffsetY`

function Dungeon:beginShifting(shiftX, shiftY)
	-- it's a rather long tween
	...
	local playerX, playerY = self.player.x, self.player.y
	-- set the player start position in the next room
	if shiftX > 0 then
		playerX = VIRTUAL_WIDTH + (MAP_RENDER_OFFSET_X + TILE_SIZE)
	elseif shiftX < 0 then
		playerX = ...
	elseif shiftY > 0 then
		playerY = ...
	else
		playerY = ...
	end
	
	Timer.tween(1, {
		-- cameraX and cameraY are basically always 0, except when shifting surely
		[self] = {cameraX = shiftX, cameraY = shiftY},
		[self.player] = {x = playerX, y = playerY}
	}):finish(function()
		-- after the tween finishes
		self:finishShifting() -- it basically set everything back to 0
		
		-- set the actual player start position, and also his direction
		if shiftX < 0 then
			self.player.x = ...
			self.player.direction = 'left'
		elseif shiftX > 0 then
			...
		elseif shiftY > 0 then
			...
		else
			...
		end
		
		-- close all doors
		for k, doorway in pairs(self.currentRoom.doorways)
			doorway.open = false
		end
		
		gSOunds['door']:play()
	end)
end
```

> [!reminder]
> We don't do the room transitions in `PlayerWalkState`, where the collision between the doors and the player happens.
> 
> But the room transitions happen in `Dungeon`, because it has access to the current room, next room, and camera, and all the stuffs that we need to perform the transition.

___

# NES Homebrew
how programming was done back in the day?

NES is written in 6502 assembly
* [NESdev Wiki](https://www.nesdev.org/wiki/Nesdev_Wiki)
* most people use CC 65 to compile 6502 assembly code

Homebrew is actually quite a popular thing
* at least amongst certain communities online
* it can compile source code to assembly for things like NES
	* which was a 8-bit microprocessor based machine
* and we can run it via an emulator
	* which allows us to run ROM images, no matter what it is

## old days

eg. [A Comprehensive Super Mario Bros. Disassembly (github.com)](https://gist.github.com/1wErt3r/4048722)
* someone took the bytes that represent all the machine code in the ROM image of Super Mario Bros., then converted it back to [[CS50x 2-1 Compiling#"Compile"|assembly language]]
* also added comments to it

* btw, it's like 14,000 lines of code
* because assembly is very, very long
	* so many steps are involved to do something
	* much more than if we would to achieve the same thing in a *high level* language like C

'80s, and early '90s games, are all in assembly language
* people program on CPU
* so maximum efficiency

For C, C++, Java, all other programming languages,
we're allowing some algorithms to do the work for us

Isn't C created in '70s? Why not use C?
* C compilers were not as good as humans were
	* at least in the context of creating games
* only human can make the processors at the time, working in 1-3 megahertz in speed
	* unlike nowadays we have 3 gigahertz processors
	* low efficiency is not a big deal now

Then '90s, when N64, PS1, PS2 came out,
things are typically done in a language like C or C++ already
* PS3 has a notoriously difficult graphics processor to program though
	* so a lot of PS3 teams needed to program in assembly
	* and it was like 2007, 2008, 2009

> [!...]
> It's a pretty enlightening experience, trying to make sense of what the assembly does.
> but it's difficult and very long and very minute too.
> 
> Definitely some insights though.

___
