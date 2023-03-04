# Entities

An entity can be almost anything you want it to be
* in our case, it's basically the living things, the player or the snails (the enemies)
* but they're like a instance of entity
* It's a very abstract thing
* they usually have their own states

Some engines may adopt an *Entity-Component System*, or ECS
* In Unity, everything is an Entity and Entities are containers of Components, and Components ultimately drive behavior

* it's like if you're familiar with *composition over inheritance*, a software engineering thing
* meaning rather than inherit a bunch of different things, we would have a base container and fill it with different components that represent the behavior
	* rather than having a base monster class > goblin class > goblin warlord class > ancient goblin warlord class > ... , so this is a nested tree of inheritance
	* we would have an entity, then a monster component, and an ancient component, and a goblin component, and a warlord component, ...
* this is all ECS is about
	* avoiding a crazy chain of inheritance being one of its benefits

But, entities, whether or not it's in a ECS like Unity or not, form the backbone of most large games
* games that have some complexity to them, model most of the pieces as entities

back to our case,
the entities, ie. the player and the snails, are separate from the tiles
* so different collision detection method
* we need to check collision on every entity with the player
	* and this is generally how we would do entity-to-entity interaction stuff
	* just iterate over every entity

```lua
--! file: GameLevel.lua
GameLevel = Class{}

function GameLevel:init(entities, objects, tilemap)
	self.entities = entities
	self.objects = objects
	self.tileMap = tileMap
end
...
function GameLevel:update(dt)
	self.tileMap:update(dt)
	
	for k, object in pairs(self.objects) do
		object:update(dt)
	end
	
	for k, entity in pairs(self.entities) do
		entity:update(dt)
	end
end

function GameLevel:render()
	self.tileMap:render(dt)
	
	for k, object in pairs(self.objects) do
		object:render(dt)
	end
	
	for k, entity in pairs(self.entities) do
		entity:render(dt)
	end
end
```

and here is an example on how to do entity-to-entity interaction
```lua
--! file: states/entity/PlayerFallingState.lua
...
	-- notice the player has a reference to the level
	-- so it can access everything within it
	for k, object in pairs(self.player.level.objects) do
		if object:collides(self.player) then
			if object.solid then
				...
			elseif object.consumable then
				...
			end
		end
	end
...
```
___

# Game Objects

In our case game objects are blocks, pick-ups, power-ups, bushes, etc.
* we don't really need them to be entities, as they don't really interact with things
	* they can be entities though, doesn't matter much
* but, they have flags and callback functions, that are gonna affect how entities interact with them
	* eg. if the player is colliding with a ladder object and press a certain key, climbs that ladder
	* eg. if the player collides with a solid block, change its current state to falling state

In our example we made living things versus non-living things as entities versus game objects, but it's just an arbitrary idea, doesn't necessarily make sense
* for a small project, these objects kinda behave the same or at least similarly, so they can all fall into this `GameObject` class
	* it's effectively inheritance
* for a larger project, there might be ladders of different types and block of different types, each behave kinda similar but not exactly the same, so they are in fact unique
	* then we should make them as entities
		* like a class for ladder, a class for block, a class for gem, etc.
		* in fact we could have a `Consumable` component for gems, a `Spawner` component for blocks to spawn things maybe like a gem, and a `Solid` or a `Collidable` component
	* it'd be a bad practice to define all those unique behaviors in a single class `GameObject`
___

# extra. declare a class

there are 2 ways to create class in Lua, using `hump.class`
1. `Class()`
2. `Class {}`

Usually we would create an instance as such
```lua
local gem = GameObject {
	texture = 'gems',
	x = (x - 1) * TILE_SIZE,
	y = (blockHeight - 1) * TILE_SIZE - 4,
	width = 16,
	height = 16,
	... ,
	onConsume = function(player, object)
		gSounds['pickup']:play()
		player.score = player.score + 100
	end
}
```

That has the effect of doing `local gem = GameObject ({...})`
* the table actually is an argument
* so really it's just one way to declare a class instance
___
