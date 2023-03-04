# Mouse Input
much like how we do with keyboard input

`love.mousepressed(x, y, key)`
`love.mousereleased(x, y, key)`
* it also has a `x` and a `y`, surely they are essential

we also have custom extended tables for `love.mouse`
* just like what we did with `love.keyboard.keysPressed` and `love.keyboard.wasPressed()`

```lua
--! file: main.lua
...
function love.load()
	...
	love.keyboard.keysPressed = {}
	love.mouse.keysPressed = {}
	love.mouse.keysReleased = {}
end
...
function love.keypressed(key)
	love.keyboard.keysPressed[key] = true
end

function love.keyboard.wasPressed(key)
	return love.keyboard.keysPressed[key]
end

function love.mousepressed(x, y, key)
	love.mouse.keysPressed[key] = true
end

function love.mousereleased(x, y ,key)
	love.mouse.keysReleased[key] = true
end

function love.mouse.wasPressed(key)
	return love.mouse.keysPressed[key]
end

function love.mouse.wasReleased(key)
	return love.mouse.keysReleased[key]
end

function love.update(dt)
	if not paused then
		gStateMachine:update(dt)
		
		love.keyboard.keysPressed = {}
		love.mouse.keysPressed = {}
		love.mouse.keysReleased = {}
	end
end
...
```

```lua
--! file: StateState.lua
...
function StartState:update(dt)
	self.world:update(dt)
	
	if love.mouse.wasPressed(1) tahen
		gStateMachine:change('play')
	end
end
...
```

so 1 represents left click
* Love2D assigns integer values to all of our mouse buttons
* 1, traditionally, is left click
	* some framework uses 0 though
___

# Start scene
an simple interesting start screen with physics

```lua
--! file: StartState.lua
...
function StartState:init()
	self.background = Background()
	self.world = love.physics.newWorld(0, 300)
	
	-- ground
	self.groundBody = love.physics.newBody(self.world, 0, VIRTUAL_HEIGHT, 'static')
	self.groundShape = love.physics.newEdgeShape(0, 0, VIRTUAL_WIDTH, 0)
	self.groundFixture = love.physics.newFixture(self.groundBody, self.groundShape)
	
	-- walls
	self.leftWallBody = love.physics.newBody(self.world, 0, 0, 'static')
	self.rightWallBody = love.physics.newBody(self.world, VIRTUAL_WIDTH, 0, 'static')
	self.wallShape = love.physics.newEdgeShape(0, 0, 0, VIRTUAL_HEIGHT)
	self.leftWallFixture = love.physics.newFixture(self.leftWallBody, self.wallShape)
	self.rightWallFixture = love.physics.newFixture(self.rightWallBody, self.wallShape)
	
	-- lots of aliens
	self.aliens = {}
	for i = 1, 100 do
		table.insert(self.aliens, Alien(self.world))
	end
end
...
```
___

# Alien

```lua
--! file: Alien.lua
function Alien:init(world, type, x, y, userData)
	self.rotation = 0
	
	self.world = world
	self.type = type or 'square' -- 'square' or 'circular'
	
	self.body = love.physics.newBody(self.world, x or math.random(VIRTUAL_WIDTH), y or math.random(VIRTUAL_HEIGHT - 35), 'dynamic')
	
	if self.type == 'square' then
		-- friends
		self.shape = love.physics.newRectangleShape(35, 35)
		self.sprite = math.random(5)
	else
		-- enemies
		self.shape = love.physics.newCircleShape(17.5)
		self.sprite = 9
	end
	
	self.fixture = love.physics.newFixture(self.body, self.shape)
	self.fixture:setUserData(userData)
end

function Alien:render()
	-- query the Box2D's Body to draw a sprite, rather than just a simple shape
	love.graphics.draw(gTextures['aliens'], gFrames['aliens'][self.sprite],
		math.floor(self.body:getX()), math.floor(self.body:getY)), self.body:getAngle(),
		1, 1, 17.5, 17.5) -- we set (17.5, 17.5) as the center of origin of the sprite
		-- to probably rotate the sprite. 17.5 because aliens are 35 by 35
end
```

`self.fixture:setUserData(userData)`
* it's a part of Fixture, so it's a part of how it resolves collisions
* and it makes the collisions to be resolved in a customized way
	* it sets arbitrary data onto a Fixture
	* eg. maybe we'd do `setUserData('alien')`, then in the Fixture there would be some kind of customized metadata saying that it's an `'alien'`, and maybe we would have `'alien'` killed on collision
* `userData` can be table as well

* basically, it's a piece of or a bunch of information that we can use at collision time
	* to perform different work

* eg. because how our Aliens resolve collisions between obstacles and other aliens and even the ground, differently
	* when I hit a ground, play a sound but don't do anything
	* when I hit a box at this velocity, destroy it
	* when I hit the alien, make it disappear and show a victory label
* so `userData` is essential
___

# Obstacle
kinda similar to Alien, as both are `'dynamic'` Bodies

```lua
--! file: Obstacle.lua
...
function Obstacle:init(world, shape, x, y)
	self.shape = shape or 'horizontal'
	
	-- similar to the 'type' of the Alien
	if self.shape == 'horizontal' then
		self.frame = 2
	elseif self.shape == 'vertical' then
		self.frame = 4
	end
	-- set x, y, world, body
	...
	if self.shape == 'horizontal' then
		self.width = 110
		self.height = 35
	elseif self.shape == 'vertical' then
		self.width = 35
		self.height = 110
	end
	
	self.shape = love.physics.newRectangleShape(self.width, self.heihgt)
	
	self.fixture = love.physics.newFixture(self.body, self.shape)
	self.fixture:setUserData('Obstacle')
end
...
```
___

# Collision Callbacks

Every collision in Box2D, triggers 4 callbacks:
1. `beginContact`
	* when 2 things begin to overlap or contact
2. `endContact`
	* when 2 objects are pushed away from each other and no longer touch each other
3. `preSolve`
	* happens right before the collision is solved, before the things get pushed away
4. `postSolve`
	* happens right after the things get pushed away from each other
	* this one gets the information about how the collision needed to resolve

We need only the `beginContact` for our game though
* all the information needed for deciding how to resolve the collisions in our game, are ready
* as soon as the 2 objects touch each other, we can just figure out how to resolve it

We implement these callbacks via a function `World:setCallbacks(f1, f2, f3, f4)`
* `f1` = `beginContact`; `f2` = `endContact`; and so forth.
* except the function names don't matter at all
	* don't have to be `beginContact`, `endContact`, `preSolve`, `postSolve`
	* just the callbacks are fired in the order of `f1 > f2 > f3 > f4`
* by default, they all are empty

```lua
--! file: Level.lua
...
function Level:init()
	self.world = love.physics.newWorld(0, 300)
	
	self.destroyedBodies = {}
	
	function beginContact(a, b, coll)
		-- a and b are the 2 objects, actually and technically, the 2 Fixtures
		local types = {}
		-- we can do this because our user datas are all strings
		-- so this is a rather simple use of user data
		types[a:getUserData()] = true
		types[b:getUserData()] = true
		
		if types['Obstacle'] and types['Player'] then
			-- if 'Player' is fast enough, destory 'Obstacle'
			if a:getUserData() == 'Obstacle' then
				local velX, velY = b:getBody():getLinearVelocity()
				local sumVel = math.abs(velX) + math.abs(velY)
				if sumVel > 20 then
					-- we can't delete Bodies and Fixtures in the middle of collision
					table.insert(self.destroyedBodies, a:getBody())
				end
			else
				local velX, velY = a:getBody():getLinearVelocity()
				local sumVel = math.abs(velX) + math.abs(velY)
				if sumVel > 20 then
					table.insert(self.destroyedBodies, b:getBody())
				end
			end
		end
		
		if types['Obstacle'] and types['Alien'] then
			-- if the obstacle is moving faster enough, kill the Alien
			...
		end
		
		if types['Player'] and types['Alien'] then
			-- if the player is moving faster enough, kill the Alien
			...
		end
		
		if types['Player'] and types['Ground'] then
			...
		end
	end
	
	function endContact(a, b, coll)
	
	end
	
	function preSolve(a, b, coll)
	
	end
	
	function postSolve(a, b, coll, normalImpulse, tangentImpulse)
		-- normalImpulse and tangentImpulse are the forces to push apart a and b
	end
	
	self.world:setCallbacks(beginContact, endContact, preSolve, postSolve)
	...
end
...
function Level:update(dt)
	...
	self.world:update(dt)
	
	-- destroy the Bodies flagged as destroyed, after self.world:update(dt)
	for k, body in pairs(self.destroyedBodies) do
		if not body:isDestroyed() then
			body:destroy()
		end
	end
	self.destroyedBodies = {}
	
	-- remove destroyed obstacles
	for i = #self.obstacles, 1, -1 do
		if self.obstacles[i].body:isDestroyed() then
			table.remove(self.obstacles, i)
			
			-- play random wood sound
			local soundNum = math.random(5)
			gSounds['break' .. tostring(soundNum)]:stop()
			gSounds['break' .. tostring(soundNum)]:play()
		end
	end
	
	-- remove destroyed aliens
	for i = #self.aliens, 1, -1 do
		if self.aliens[i].body:isDestroyed() then
			...
		end
	end
end
```

we implemented all 4 callbacks even though we just used 1
* because we might want to use them later
* eg. maybe we have 2 objects attached on collision, as if they're magnetic or something, and when they're pull after, (`endContact` happens), we want a particle effect to show
* eg. use `postSolve` because maybe we want something to fly off on collision

also notice we have functions inside of a function
* 4 callback functions inside of the `Level:init()`
* it's a little bit hairy, but we don't have many lines of code for the callbacks so ....

> [!info]
> What collides? Bodies or Fixtures?
> 
> Fixtures collide with each other, but not the Bodies. Recall Bodies are just position and velocity containers. That's why we do `Fixture:setUserData()` instead of `Body:setUserData()`.

Also, why `self.destroyedBodies` and `table.insert(self.destroyedBodies, Body)`?
* Box2D maintains a reference to all of the Bodies and Fixtures, in the World
	* regardless of whether we deleted them or not
* but if we delete them in the middle of a collision, we're making Box2D to do collision between something and nothing
	* results in a crash or a stack overflow
* so don't ever delete or destroy anything while inside a collision callback
	* instead we reference the things we're going to destroy
	* and after the world updates, we can destroy them
* basically don't delete in the middle of World update

Then why are we destroying Bodies but not Fixtures?
* when we destroy Bodies, it destroys all the associated Fixtures as well
* so Body is a top level container, containing Fixtures
	* if we have a Body with 5 Fixtures, destroying the Body will also destroy all 5 of its Fixtures
	* destroying any 1 of 5 its Fixtures, will just destroy that specific Fixture, as oppose to destroying the Body or all 5 Fixutres
___

# Launch Alien
this is where we launch or sling shot the alien
* click and drag the alien to aim, and release the mouse to launch
	* launch an alien with a Body and Fixture
* also renders a trajectory to foretell the shot

We shoot the alien by giving it an *impulse*
* it's just effectively setting its velocity immediately to some value
* as opposed to setting its velocity over time, that is to say, via *force*

* eg. a car gradually accelerating, that's a force
	* same amount of force applying to the car, making it accelerate gradually
* eg. a car hit a wall in some speed, that's an impulse being applied to the wall
* when we release the alien, we want an impulse of some amount in the opposite direction in where we're dragging, applied to the alien

```lua
--! file: AlienLaunchMarker.lua
function AlienLaunchMarker:init()
	...
	self.aiming = false
	self.launched = false
	
	self.alien = nil
end

function AlienLaunchMarker:update(dt)
	if not self.launched then
		-- graph mouse coordinates
		local x, y = push:toGame(love.mouse.getPosition())
		
		if love.mouse.wasPressed(1) and not self.launched then
			self.aiming = true
		elseif love.mouse.wasReleased(1) and self.aiming then
			-- launch the alien
			self.launched = true
			
			-- spawn an Alien
			self.alien = Alien(self.world, 'round', self.shiftedX, self.shiftedY, 'Player')
			-- baseX and baseY are the start position before we move the alien
			self.alien.body:setLinearVelocity((self.baseX - self.shiftedX) * 10, (self.baseY - self.shiftedY) * 10)
			
			self.alien.fixture:setRestitution(0.4)
			self.alien.body:setAngularDamping(1) -- this is the friction on rotation
			-- so that it can stop rolling on the ground at some point
			-- self.alien.body:setAngularDamping(1) is roll forever and ever and ever
			
			self.aiming = false
			
		elseif self.aiming then
			-- they're the start position after we launch the alien
			-- where we spawn the alien and apply the impulse
			self.shiftedX = math.min(self.baseX + 30, math.max(x, self.baseX + 30))
			self.shiftedY = math.min(self.baseY + 30, math.max(y, self.baseY + 30))
		end
	end
end

function AlienLaunchMarker:render()
	-- not launched and aimming yet
	love.graphics.draw(gTextures['aliens'], gFrames['aliens'][9],
		slef.shiftedX - 17.5, self.shiftedY - 17.5)
	
	if self.aiming then
		-- aiming, render the trajectory
		...
		-- calculate thr trajectory
		-- http://www.iforce2d.net/b2dtut/projected-trajectory
		for i = 1, 90 do -- calculate only the first 90 1/60 of a second, = 1.5 second
			love.graphics.setColor(255, 80, 255, (255 / 12) * i)
			
			-- self.shiftedX and self.shiftedY are the launch position
			-- trajX and trajY are the projected position in that iteration
			trajX = self.shiftedX + i * 1 / 60 * impulseX
			trajY = self.shiftedY + i * 1 / 60 * impulseY + 0.5 * (i * i + i) * gravY * 1 / 60 * 1 / 60
			if i % 5 == 0 then
				love.graphics.circle('fill', trajX, trajY, 3)
			end
		end
		
		love.graphics.setColor(255, 255, 255, 255)
	else
		self.alien:render()
	end
end
```
___

# Level

## init

```lua
--! file: Level.lua
function Level:init()
	...
	self.launchMarker = AlienLaunchMarker(self.world)
	
	self.aliens = {}
	self.obstacles = {}
	self.edgeShape = love.physics.newEdgeShape(0, 0, VIRTUAL_WIDTH, 0) -- for ground
	
	table.insert(self.aliens, Alien(self.world, 'square', ...)) -- some x and some y
	
	table.insert(self.obstacles, Obstacle(self.world, 'vertical',
		VIRTUAL_WIDTH - 120, VIRTUAL_HEIGHT - 35 - 110 / 2))
	table.insert(self.obstacles, Obstacle(self.world, 'vertical',
		VIRTUAL_WIDTH - 35, VIRTUAL_HEIGHT - 35 - 110 / 2))
	table.insert(self.obstacles, Obstacle(self.world, 'horizontal',
		VIRTUAL_WIDTH - 80, VIRTUAL_HEIGHT - 35 - 110 - 35 / 2))
	
	self.groundBody = love.physics.newBody(self.world, -VIRTUAL_WIDTH, VIRTUAL_HEIGHT)
	self.groundFixture = love.physics.newFixture(self.groundBody, self.edgeShape)
	self.groundFixture:setFriction(0.5)
	self.groundFixture:setUserData('Ground')
	
	self.background = Background() -- set background graphics
end
```

and then if we want to apply data driven data to create levels
```lua
level = {
	[1] = {
		['aliens'] = {
			{
				x = --
				y = --
			}
		},
		['obstacles'] = {
			{ ... },
			{ ... },
			...
		}
	}
}
```

kinda like JSON in JavaScript
* we can define things as data, then write a script to go over it and construct the actual relevant data structures

> [!info]
> Remember not only it's easier to create content as data, we can shift the burden to somebody else who may not be as comfortable in programming.

## update

```lua
function Level:update(dt)
	self.launchMarker:update(dt)
	self.world:update(dt)
	
	-- destroy Box2D Bodies
	for k, body in pairs(self.destroyedBodies) do
		if not body:isDestroyed() then
			body:destroy()
		end
	end
	-- clear destroy flags
	self.destroyedBodies = {}
	
	-- delete objects from the game
	for i = #self.obstacles, 1, -1 do
		if self.obstacles[i].body:isDestroyed() then\
			table.remove(self.obstacles, i)
			
			local soundNum = math.random(5)
			gSounds['break' .. tostring(soundNum)]:stop()
			gSounds['break' .. tostring(soundNum)]:play()
		end
	end
	
	for i = #self.aliens, 1, -1 do
		if self.aliens[i].body:isDestroyed() then
			table.remove(self.aliens, i)
			gSounds['kill']:stop()
			gSounds['kill']:play()
		end
	end
	
	if self.launchMarker.launched then
		-- if alien launched
		local xPos, yPos = self.launchMarker.alien.body:getPosition()
		local xVel, yVel = self.launchMarker.alien.body:getLinearVelocity()
		
		if xPos < 0 or (math.abs(xVel) + math.abs(yVel) < 1.5)
			-- if the alien fired is almost stopped, rather than completely still
			-- because it's tideous to wait that long
			
			-- destroy the alien
			self.launchMarker.alien.body:destroy()
			-- create a new LaunchMarker, which will handle creating a new alien
			self.launchMarker = AlienLaunchMarker(self.world)
			
			if #self.aliens == 0 then
				-- if there is no more aliens, go back to the start screen
				gStateMachine:change('start')
				-- this isn't call right when the last alien is killed because
				-- we need to wait for the launched alien to stop rolling
			end
		end
	end
end
```

## render

```lua
function Level:render()
	-- render ground tiles
	for x = -VIRTUAL_WIDTH, VIRTUAL_WIDTH * 2, 35 do
		love.graphics.draw(...)
	end
	... -- defer rendering to all sort of objects in the level
	
	-- also some UI stuffs
	if not self.launchMarker.launched then
		-- if we didn't launch the alien yet
		love.graphics.setFont(gFonts['medium'])
		love.graphics.setColor(0, 0, 0, 255)
		love.graphics.printf('Click and drag circular alien to shoot!',
			0, 64, VIRTUAL_WIDTH, 'center')
		love.graphics.setColor(255, 255, 255, 255)
	end
	
	if #self.aliens == 0 then
		-- if there is no aliens left in the game
		love.graphics.setFont(gFonts['huge'])
		love.graphics.setColor(0, 0, 0, 255)
		love.graphics.printf('VICTORY', 0, VIRTUAL_HEIGHT / 2 VIRTUAL_WIDTH / 2)
		love.graphics.setColor(255, 255, 255, 255)
	end
end
```
___

# More features

* more obstacle shapes
* compound obstacles
	* eg. see pulley, motor, weld joints, ....
* Levels defined as tables (data-driven)
* Birds with different shooting mechanics
* Obstacle with different materials and different densitites
___
