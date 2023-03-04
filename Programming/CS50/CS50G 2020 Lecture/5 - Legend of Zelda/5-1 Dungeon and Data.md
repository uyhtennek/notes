# Legend of Zelda
much like Mario, it's a very iconic game as well

even todays, Breath of The Wild (2017) basically took every award possible
* the original Legend of Zelda was an NES title
	* just like Super Mario Brothers
* it's tile based as well, like most games of its time
* the goal of the game is sort of explore this open world
	* which is kind of a first of its kind

Links, the main character, has a sword, bombs, arrows
* he went to find rubies
* went through dungeons
* slayed monsters and bosses
* the ultimate goal is to obtain the Triforce

![Level-1_entrance_room_-_TLOZ_NES.png (256×224) (zeldadungeon.net)](https://www.zeldadungeon.net/wiki/images/b/ba/Level-1_entrance_room_-_TLOZ_NES.png)

In every dungeon, we got
* a map layout, maps are arranged in a grid
	* we can go room by room, where each room has the size of the entire screen
	* some have puzzles, some have items, monsters, ....
* hearts are at the very top right
	* when we take damage, hearts sort of decrement

An interesting thing happens in NES is that when Link going from one room to another, there is a transition period or loading period as the camera pan from one room to another
* sort of an iconic thing in Zelda
* unlike Löve, in NES it doesn't have a world coordinate, everything happens inside of the screen
	* the transition is done by dynamically overwriting tiles column by column
	* nowadays memory isn't really an issue so we'd adopt another approach
___

# Sprite Structure

## Tiles

First of all when we're doing our sprite arts, it's a good practice to structure the assets in a way that they can be easily chopped up to tile pieces
* meaning make everything 1 tile big, 2 tiles big, 3 tiles big. But not 1.5 tile big or 1.2 tile or else
* so we can index into the sprite via some table of quads

If we have something that is bigger than 1 tile, how to index it or draw it?
* just index any one of its tile, probably the top left one
* we would reference its size in some way, so instead of drawing just 1 tile, we draw multiple tiles
	* something like this object is 3 tiles wide and 2 tiles tall
* same goes for its collision box
	* we can do a little bit of customization to the collision box too since it doesn't necessary need to be the same size as all its tiles

* It's a key to tile based games
	* otherwise it's hard to implement things like big trees, houses, big doors, etc.


## Animation Sprites

Next we have the character sprite
* it has padding. meaning the arts are not divided neatly into even segments.
* it has a number of set of animations, but some animations are of different sizes

In our case the character size is 16 by 20 pixels
* most of its animations are also framed as 16 by 20
* but, it has a sword swing animation set, which is 32 by 32 pixels big
	* Why? because 16 by 20 doesn't have enough area for drawing the sword

So here is the problem, when the character is swinging his sword, the sprite is 32 by 32, but the character should be still 16 by 20 in size
* if the character's collision box starts at its `(0, 0)`, 16 by 20 isn't quite where the character is in the sprite right?

The solution is that we need to assign the sprite an offset
* kinda like the character's collision box still starts at `(0, 0)`, but the sprite is drawn at `(-x, -y)`
* that way we can shift the sprite by some amount so it aligns with the character

Lastly, for sprites that contain quite a number of animation, it's better to mark down the animation frames IDs
* probably a great place to apply some Python skills


## Top-Down Perspective

One more thing, so top-down isn't actually totally top-down
* In Zelda, everything is slightly tilted
* also there are shadows and highlights, quite important for convincing graphics
	* like the 4 walls in a room should have different colors of shadow
	* probably the top one is quite bright and the bottom one is quite dark
* make sure to do corners at well, to maintain the illusion
___

# Dungeon Generation

> [!info]
> In Zelda, in most games as well, dungeons are set in advance by the designers.
> In some games, like The Binding of Isaac, dungeons can also be generated.

First of all, what is the most foundamental unit of a dungeon?
* Rooms

![Eagle-Map.png (1536×1056) (zeldadungeon.net)](https://www.zeldadungeon.net/Zelda01/Walkthrough/02/Eagle-Map.png)

Looking at the map of a dungeon, we can almost picture it in terms of a 2D array, right?
```
off,  on,  on, off, off, off,
off, off,  on, off,  on,  on,
...
```

Also the rooms have connections to other rooms
* 2 rooms next to each other don't necessarily have a connection though

* Then how do we go from one room to another
	* either the x or y increases or decreases 1
* and we load the room accordingly, then perform the transition, then set that to the current room

Be careful when designing the dungeons though, since we maybe doing things procedurally
* say we want a door in a room locked
* make sure the key isn't in the other side

> [!warning]
> Although we're learning to do things in a large part, randomly, we need to have some conscious design on behalf of our algorithms.

**Tips**
maybe have a graph, of control flow, representing the dungeon and the progression thereof, to aid your design. Just make sure there are puzzles or obstacles, that players can solve.


## eg. The Binding of Isaac

* screen size is the room size
* have a mini-map of rooms
	* show the important rooms
	* a locked room and a boss room

Isaac actually generate keys and bombs randomly
* the designer didn't plant the key in some specific locations
* maybe like at the end of every room, have a random chance to spawn things
	* need to make sure the end or exit of the dungeon isn't behind the locked door though
* so if the player is not lucky, they can't skip the boss
	* therefore Isaac doesn't lock the boss room

So this is a great example of a great randomisation
* using a number of principles
* ultimately produce a game that is extremely addicting and fun

kinda like an opposite end of The Legend of Zelda
* which is super elaborate
___

## Level structure

Firstly notice, we have everything that is related to the game world, in a `world/` folder
* `Dungeon.lua`, `Room.lua`, `Doorway.lua`

* The main idea is that, we divide the game world into subsets of the level
	* Dungeon is the game world, Room is the levels in the world, Doorway is the doors in the each level
	* maybe we have a platformer level, maybe we have some regions or zones, little subsegments of the level that we can transition between

It's useful because of efficiency and performance
* we can load dynamically certain aspects, certain components, sub areas of the level, one at a time, rather than just the entire level at once
	* unless you want to exercise your computer's memory constraints
___

# Our Dungeon Code

```lua
--! file: Dungeon.lua
Dungeon = Class{}

function Dungeon:init(player)
	self.player = player
	self.rooms = {}
	
	self.currentRoom = Room(self.player)
	self.nextRoom = nil
	...
end
```

`Room(self.player)`
* it's important to have the player to have access to all the entities and objects in a room
* because we need to do things like collision detection, and have the entities in the room to look at the player and then decide what their AI does

Next, what a room looks like? What it should keep track of?
* doors, player, objects like the switch, entities like the creatures
* A set of tiles
	* floor, 4 walls, 4 corners

```lua
--! file: Room.lua
Room = Class{}

function Room:init(player)
	self.width = MAP_WIDTH
	self.height = MAP_HEIGHT
	
	self.tiles = {}
	self:generateWallsAndFloors()
	
	self.entities = {}
	self:generateEntities()
	
	self.objects = {}
	self:generateObjects()
	
	self.doorways = {}
	-- we have static, rather simple doors, so didn't create a function for them
	-- so always 4 doors at each wall
	table.insert(self.doorways, Doorway('top', false, self))
	table.insert(self.doorways, Doorway('bottom', false, self))
	table.insert(self.doorways, Doorway('left', false, self))
	table.insert(self.doorways, Doorway('right', false, self))
	
	self.player = player
	
	self.renderOffsetX = MAP_RENDER_OFFSET_X
	self.renderOffsetY = MAP_RENDER_OFFSET_Y
	
	self.adjacentOffsetX = 0
	self.adjacentOffsetY = 0
end
...
```

`Doorway('top', false, self)`
* the direction is important, which we will later see
* `false` means the door is closed
* we passed `self` so the door has access to the Room class

`self.renderOffsetX = MAP_RENDER_OFFSET_X`
* since the tiles don't completely match up to the width and height of the screen
	* `VIRTUAL_HEIGHT` is not dividable by 16 by 16 tiles
* we shifted everything inwards
	* 1 tile in width and 1 tile in height, smaller than the screen
	* also since the door sprites have a half-tile padding in it so the dungeon is a little bit smaller than `VIRTUAL_WIDTH - 16` and `VIRTUAL_HEIGHT - 16`
* and then offset it by a certain amount so that it's centered
	* so we shifted the room by half of the pixels that are left between the screen size and the fully rendered dungeon size
	* also this is how all kind of 'center to the screen' works
		* screen size - size of the thing, divided by 2

`self.adjacentOffsetX = 0`
* it's for drawing the room, when it's the adjacent room
* when we load the next room, we instantiated the room, but we don't want to draw it at `(0, 0)`, we want to draw at right next to our current room


## Room transition

1. we're in `self.currentRoom` (in the Dungeon class) and walks on a door way
2. we spawn a room, store it as `self.nextRoom`
	* before this, it's `nil`
3. depending on which direction we're moving in, which door that we walked in to, we set the next room's `self.adjacentOffsetX` and `self.adjacentOffsetY` accordingly
4. for the transition we would tween the camera, `love.graphics.translate()`
	* `(0, 0)` to `(adjacentOffsetX, adjacentOffsetY)`
5. finally when the transition is done, `self.currentRoom = self.nextRoom`
	* set `adjacentOffsetX` and `adjacentOffsetY` to 0
	* so room is at `(0, 0)` again, so set the camera back to `(0, 0)`

* we went up when the transition is happening
	* the room is up there, the camera is going towards there
* when the transition finished, everything is instantly back to `(0, 0)`
	* along with the player, entities, objects in that room


## Room generation

```lua
--! file: Room.lua
...
function Room:generateWallsAndFloors()
	-- similar to the TileMap in the Mario lecture
	for y = 1, self.height do
		table.insert(self.tiles, {})
		for x = 1, self.width do
			local id = TILE_EMPTY
			
			-- 4 corners
			if x == 1 and y == 1 then
				id = TILE_TOP_LEFT_CORNER
			else if x == 1 and y == self.height then
				id = TILE_BOTTOM_LEFT_CORNER
			else if x == self.width and y == 1 then
				id = TILE_TOP_RIGHT_CORNER
			elseif x == self.width and y == self.height then
				id = TILE_BOTTOM_RIGHT_CORNER
			
			-- 4 walls
			elseif x == 1 then
				id = TILE_LEFT_WALLS[math.random(#TILE_LEFT_WALLS)]
			elseif x == self.width then
				id = TILE_RIGHT_WALLS[math.random(#TILE_RIGHT_WALLS)]
			...
			
			-- floors
			...
			end
			
			table.insert(self.tiles[y], {
				id = id
			})
		end
	end
end
```

Recall we can give the tile in our sprite sheet an ID, then we can then index into and draw the specific frame
* it's not the only way
* but it's the easiest thing to do
	* a simple, lightweight, clean approach of modeling

Notice, although we can directly put the ID number there, we prefer constant
* like `TILE_TOP_LEFT_CORNER`, `TILE_BOTTOM_LEFT_CORNER`, etc.
* why? because for readability
	* hard to understand `id = 42`, `id = 43`, ...


## Generate entities

```lua
--! file: Room.lua
...
function Room:generateEntities()
	local types = {'skeleton', 'slime', 'bat', 'ghost', 'spider'}
	
	for i = 1, 10 do
		local type = types[math.random(#types)]
		table.insert(self.entities, Entity {
			animations = ENTITY_DEFS[type].animations,
			walkSpeed = ENTITY_DEFS[type].walkSpeed or 20,
			
			x = math.random(MAP_RENDER_OFFSET_X + TILE_SIZE,
				VIRTUAL_WIDTH - TILE_SIZE * 2 - 16),
			y = math.random(MAP_RENDER_OFFSET_Y + TILE_SIZE,
				VIRTUAL_HEIGHT - (VIRTUAL_HEIGHT - MAP_HEIGHT) + ...),
			
			width = 16,
			height = 16,
			health = 1
		})
	end
end
```

`ENTITY_DEFS`
* a global table
* we put what matters for each individual entity there
	* their characteristics

```lua
--! file: entity_defs.lua
ENTITY_DEFS = {
	['player'] = {
		walkSpeed = PLAYER_WALK_SPEED,
		animations = {
			['walk-left'] = {
				frames = {13, 14, 15, 16},
				interval = 0.155,
				texture = 'character-walk'
			},
			['walk-right'] = {
				frames = {5, 6, 7, 8},
				interval = 0.15,
				texture = 'character-walk'
			},
			['walk-down'] = { ... },
			['walk-up'] = { ... },
			['idle-left'] = { ... },
			...
		}
	},
	['skeleton'] = {
		texture = 'entities',
		animations = {
			['walk-left'] = {
				frames = {22, 23, 24, 23},
				interval = 0.2
			},
			['walk-right'] = { ... },
			...
		}
	},
	...
}
```

* Everything is clean data
	* There is no logic here
	* Just flags, values, simple things

It's important to complex games or complex systems,
that we can describe the entities as data
* then just let the engine parse these information
* then create the entities programmatically

* kinda shift the burden from the programmer to the designer, a little bit
* so people who don't know programming can still modify the game
	* add things, change chings, etc.
	* without even looking at the game code

Here the entities are fairly simple
* in a more complex example maybe we'll have many many attributes
* is it flammable? how much is its health? what are the animations? what skills it have? its attack strength? its defense? where does it spawn?
* is this entity a weapon? an item? an ability? levels even? anything we want to.

But everything is solely, data.


### eg. RPGs

this design is very essential to complex games
* RPG being a rather complex genre

eg.
* skills can have particle effects
* many things can do different damages to different things
* some entities can be set on fire, or electrocuted, but some don't
* some entities can melt when they touch something

but it all boil down to a bunch of flags, and a bunch of functions that parse these
* incredibly boost productivity

We wouldn't need many classes
* like a Spider class, a Ghost class, a Bat class, etc. It is unnecessary.
* all we need is to define what attributes does a Spider have? a Ghost have? a Bat have?

* as a result anybody can mod the game
	* they just need to do what attributes a potential entity should have and can have
* your design team would be all the more productive

So when we look back in [[#Generate entities|`Room:generateEntities()`]]
* notice how it pull the definitions from `ENTITY_DEFS` and parse out each individual relevant data
* then it can just construct some relevant information for the desired behavior
	* like attach a relevant flag to that entity

eg. flammable
1. if we do a fire attack, or fire type attack
2. if it collides with an entity, that its `flammable` is true
3. set it on fire

so it's not terribly complex
and we can assign this behavior to any entity

eg. goblin
```lua
['goblin'] = {
	health = 10,
	strength = 2,
	texture = 'goblin',
	animations = {
		['idle'] = {
			frames = {1},
			interval = 1
		},
		['walking-left'] = {
			frames = {2, 3, 4, 2},
			interval = 0.2
		}
	},
	-- maybe the game references to a `WEAPON_DEFS` to find this 'club' weapon
	-- and each weapon has a bunch of different characteristics or fields as well
	weapon = 'club',
	-- maybe some enemies will actively chase the player, some just docile
	aggressive = true,
	-- maybe some entities go to a SleepState when it's nighttime
	sleepsAtNight = true,
	-- maybe when some entities touch fire, or lava, get set on fire
	-- and take damage over time, and maybe get tinted red
	flammable = true
}
```

> [!summary]
> Modeling complex behaviors of entities, objects, whatever, as data. Rather than thinking about it in terms of classes.
> 
> So no goblin class, skeleton class, something class, ....
> Entities no matter what it is, are just composed with attributes, and then modelled to behave in some way accordingly.
> 
> Composition over inheritance.

> [!Btw]
> That's the Unity game engine approach as well, doing ECS, *entity component system*.
> Entities are modeled as collections of components that each do something.


## Generate objects

notice the code is fairly short and easy to understand
* but the switch can already do chaning its sprite and opening doors, and implicitly changing the look and behavior of the doors as well
```lua
--! file: Room.lua
...
function Room:generateObjects()
	table.insert(self.objects, GameObject(
		GAME_OBJECT_DEFS['switch'],
		math.random(MAP_RENDER_OFFSET_X + TILE_SIZE,
			VIRTUAL_WIDTH - TILE_SIZE * 2 - 16),
		math.random(MAP_RENDER_OFFSET_Y + TILE_SIZE,
			VIRTUAL_HEIGHT - (VIRTUAL_HEIGHT - MAP_HEIGHT * TILE_SIZE))
	))
	
	local switch = self.objects[1]
	
	switch.onCollide = function()
		-- when the switch collided with something (the player)
		if switch.state == 'unpressed' then
			switch.state = 'pressed'
			
			for k, doorway in pairs(self.doorways) do
				-- open every door
				doorway.open = true
			end
			
			gSounds['door']:play()
		end
	end
end
```

notice there is this definition global table again
```lua
--! file: game_objects.lua
GAME_OBJECT_DEFS = {
	['switch'] = {
		type = 'switch',
		texture = 'switches',
		frame = 2,
		width = 16,
		height = 16,
		solid = false,  -- player can walk over it
		defaultState = 'unpressed',
		states = {
			['unpressed'] = {
				frame = 2
			},
			['pressed'] = {
				frame = 1
			}
			-- we can add more state if we need as well, so infinite flexibility
		}
	},
	['pot'] = {
		...
	},
	...
}
```


## Room update & render

```lua
--! file: Room.lua
...
function Room:update(dt)
	if self.adjacentOffsetX ~= 0 or self.adjacentOffsetY ~= 0 then
		return
	end
	
	-- the following is quite similar to the Mario lecture
	self.player:update(dt)
	
	-- update the entities
	for i = #self.entities, 1, -1 do
		local entity = self.entities[1]
		
		-- if health is <= 0, it's dead
		if entity.health <= 0 then
			entitiy.dead = true
		elseif not entity.dead then
			entity:processAI({room = self}, dt)
			entity:update(dt)
		end
		
		if not entity.dead and self.player:collides(entity) and not self.player.invulnerable then
			-- damage the player and go invulnerable
			gSounds['hit-player']:play()
			self.player:damage(1) -- it just subtract player's health
			self.player:goInvulnerable(1.5) -- invulnerable for 1.5 seconds
			
			if self.player.health == 0 then
				-- player die
				gStateMachine:change('game-over')
			end
		end
	end
	
	for k, object in pairs(self.objects) do
		object:update(dt)
		if self.player:collides(object) then
			object:onCollide()
		end
	end
end

function Room:render()
	-- draw the tiles
	for y = 1, self.height do
		for x = 1, self.width do
			local tile = self.tiles[y][x]
			love.graphics.draw(gTextures['tiles'], gFrames['tiles'][tile.id],
				(x - 1) * TILE_SIZE + self.renderOffsetX + self.adjacentOffsetX,
				(y - 1) * TILE_SIZE + self.renderOffsetY + self.adjacentOffsetY)
		end
	end
	
	-- render the doorways
	for k, doorway in pairs(self.doorways) do
		doorway:render(self.adjacentOffsetX, self.adjacentOffsetY)
	end
	
	-- render the objects
	for k, object in pairs(self.objects) do
		object:render(self.adjacentOffsetX, self.adjacentOffsetY)
	end
	
	-- render the entities
	for k, entity in pairs(self.entities) do
		-- if it's not dead yet
		if not entitiy.dead then
			entity:render(self.adjacentOffsetX, self.adjacentOffsetY)
		end
	end
	
	-- we'll discuss this later
	love.graphics.stencil(function()
		...
	end, 'replace', 1)
	love.graphics.setStencilTest('less', 1)
	
	-- render the player
	if self.player then
		self.player:render()
	end
	
	love.graphics.setStencilTest()
end
...
```

notice as well, when we model the entities and attributes like the above
* the code is actually quite readable
	* eg. `entity.dead`, `self.player.invulnerable`

Again, behaviors are defined by just flags
* check the flag, then change the rendering, change the mechanics of the entity accordingly
___

# Stenciling

When the player walk into a door and trigger a room switch
* Notice it looks as if the character walks underneath the tiles, or the door
* But, we're drawing the tiles **before** the player

One approach that we might think of is
* "Okay, I'll just render the player before I render the doorways"
* but this doesn't work out, why?
	* the player, or some part of the player, would disappear as soon as it get to the door
	* the player sprite is getting cut off
	* happens on other entities as well

* because the door tiles are rendered after the player
	* although some part of the door tiles is in fact the floor

The solution is what's called a **stencil**
* a stencil is any arbitrary shape that we want
* that's invisible, but get drawn onto the screen somehow
* it determines whatever gets drawn on top of the stencil, should gets rendered or not

In our example, the stencils are 'behind' the door, towards the outside of the screen
* then we say, "whatever passes through the stencils during the stencil testing period, if it's on the stencils, don't render it"
	* during the stencil testing period we only drew the player
* so we still draw the doorways before the player, but as soon as it hits the stencil, it is not drawn

Technically though, it's drawing the player to the stencil
* but not the actual canvas

That is an approach to get convincing layered visual effects

```lua
--! file: Room.lua
...
function Room:render()
	...
	love.graphics.stencil(function()
		-- left door
		love.graphics.rectangle('fill', -TILE_SIZE - 6, MAP_RENDER_OFFSET_Y +
			TILE_SIZE * 2 + 6, TILE_SIZE * 2)
		-- right door
		...
		-- top door
		...
		-- bottom door
		...
	end, 'replace', 1)
	-- we drew a several rectangles as stencils,
	-- all the pixels we drew here, are 'replaced' with a stencil value of '1'
	
	love.graphics.setStencilTest('less', 1)
	
	if self.player then
		-- player has a value of 1, so it's not less than 1, so don't draw it
		self.player:render()
	end
	
	love.graphics.setStencilTest()
end
```

> [!tips]
> Anything that didn't get ascribed a value, get a stencil value of 1.
> And stencil value is like a hidden value, that is used to determine the behaviors of some pixels in some way.

`love.graphics.stencil(func, [action], [value], [keepvals])`
* performs all stencil drawing within `func`
	* things drawn there will act as the stencil pixels, during `love.graphics.setSencilTest()`
* `action` defines how those pixels will behave
	* if the values of the things we draw later match some kind of condition regard to `value`

`love.graphics.setStencilTest(compare_mode, compare_value)`
* compares pixels drawn via `compare_mode` with `compare_value`
* only draw the pixels if the comparison holds true
* so `love.graphics.setStencilTest('less', 1)` means
	* when things overlap with the stencils that we defined or drew in `love.graphics.stencil()`
	* we'll look at its values to see if it is `'less'` than `1`
		* if it is, draw it
		* if it isn't, don't draw it

* things that are not overlapping with the stencils will still be drawn no matter its value
* there are different comparisons
	* `'less'`, `'greater'`
	* maybe we would do iterative stenciling
		* for each iteration the value to check gets higher and higher, so less things get drawn
* stencil values can go between 0 and 255

> [!tip]
> If we want to see where the stencil rectangles are, just take them out from the `love.graphics.stencil()` function.

by the way, we can choose not to use stencil
* like seperating the door sprite to 2 sprites
	* one for the shadow underneath the door; and
	* one for the door
* then draw the shadow first, then the player, then the door


## lighting?
stencil can be any shape, so maybe we can say like some areas are lighter, some are darker?

possibly can, possibly can't. Not a bright idea anyway.
* usually stencil works kinda like on and off
* not sure if it can do gradient

if you're interested in lighting
* look for *faux lighting system*
	* allows us to use a lighting kit, like Box 2D Lights

a simpler and cruder way to do lighting is drawing a shape, that to be our darkness, and then render it in semi-transparent
* that way we can see it gets darker
* and we can still see all the things behind or below of the semi-transparent shape
	* we can even apply gradient to the shape too
___
