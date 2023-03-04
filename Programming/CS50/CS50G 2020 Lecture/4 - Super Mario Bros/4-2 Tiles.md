# Procedural Level Generation

a very basic way to genearte a platformer level, is to
* generate the level, column by column
* by default, the ground should be a certain height
	* in some columns, we want the ground is at a different height
	* in some columns, we don't want it to generate a ground at all

the art of procedural level generation though, is how to
* create obstacles and interesting levels and scenery for the player

another benefit is that the arts for a platformer game is fairly easy to make
* so procedural level generation can take advantage of that to create a vast variety of scenery
* although there isn't many platformers that do procedural level generation
___

# level-0: Flat Levels

here we want to generate not only the ground, but also the topper
* no pillar or chasm yet

how it's done is that we would have a flag in the tiles
* that tells whether or not it has a topper on it
* if it has, after it renders the tile, it also renders the topper

one more thing, the sprite for the levels and toppers is quite big
* containing many arts of many type of platforms or toppers of many level themes
* so it's better to store each tile set in its own table
* and if we want the code to be easy and clean
	* it relies on us or the artist to cleanly set up the sprite sheet in a way that the tiles are arranged in a neat and tidy order

```lua
...
TILE_SET_WIDTH = 5
TILE_SET_HEIGHT = 4

TILE_SETS_WIDE = 6
TILE_SETS_TALL = 10

TOPPER_SETS_WIDE = 6
TOPPER_SETS_TALL = 18
...
function love.load()
	tilesheet = love.graphics.newImage('tile_tops.png')
	-- every single tile in the big tile sheet, put in one table
	quads = GenerateQuads(tilesheet, TILE_SIZE, TILE_SIZE)
	
	topperSheet = love.graphics.newImage('tile_tops.png')
	topperQuads = GenerateQuads(topperSheet, TILE_SIZE, TILE_SIZE)
	
	-- divide the big quads table into several tile sets table
	tilesets = GenerateTileSets(quads, TILE_SETS_WIDE, TILE_SETS_TALL, TILE_SETS_WIDTH, TILE_SET_HEIGHT)
	toppersets = GenerateTileSets(topperQuads, TOPPER_SETS_WIDE, TOPPER_SETS_TALL, TILE_SET_WIDTH, TILE_SET_HEIGHT)
	
	-- randomly pick one tileset
	tileset = math.random(#tilesets)
	topperset = math.random(#toppersets)
	...
end

function love.keypressed(key)
	if key == 'r' then
		tileset = math.random(#tilesets)
		topperset = math.random(#toppersets)
	end
end
...
function love.draw()
	...
	for y = 1, mapHeight do
		for x = 1, mapWidth do
			local tile = tiles[y][x]
			love.graphics.draw(tilesheet, tilesets[tileset][title.id],
				(x - 1) * TILE_SIZE, (y - 1) * TILE_SIZE)
			
			if tile.topper then
				love.graphics.draw(topperSheet, toppersets[topperset][tile.id],
					(x - 1) * TILE_SIZE, (y - 1) * TILE_SIZE)
			end
		end
	end
	...
end

function generateLevel()
	-- generating a flat level, so much like the previous love.draw() function
	local tiles = {}
	
	for y = 1, mapHeight do
		table.insert(tiles, {})
		for x = 1, mapWidth do
			table.insert(tiles[y], {
				id = y < 7 and SKY or GROUND,
				topper = y == 7 and true or false -- except we added a topper flag
			})
		end
	end
	
	return tiles
end
```
___

# level-1: Pillar Levels

```lua
...
function generateLevel()
	local tiles = {}
	
	-- populate the entire level as empty
	-- so later we just change the tile instead of doing insertion
	for y = 1, mapHeight do
		table.insert(tiles, {})
		for x = 1, mapWidth do
			table.insert(tiles[y], {
				id = SKY,
				topper = false
			})
		end
	end
	
	for x = 1, mapWidth do
		-- 1/5 chance to spawn a pillar
		local spawnPillar = math.random(5) == 1
		
		if spawnPillar then
			-- spawn the pillar
			for pillar = 4, 6 do
				tiles[pillar][x] = {
					id = GROUND,
					-- the top most ground needs a topper
					topper = pillar == 4 and true or false
				}
			end
		end
		
		for ground = 7, map Height do
			tiles[ground][x] = {
				id = GROUND,
				-- draw the topper where the surface of the ground is
				topper = (not spawnPillar and ground == 7) and true of false
			}
		end
	end
	
	return tiles
end
```

* once randomization comes into play, sometimes the level generated can surprise even the person who wrote the algorithm
* it saves us work having to create levels as well
___

# level-2: Chasmed Levels

```lua
...
function generateLevel()
	... -- populate an entirely empty level
	for x = 1, mapWidth do
		-- 1/7 chance to not spawn anything on this column
		-- 7 is a static magic number, magic numbers are generally bad though
		if math.random(7) == 1 then
			goto continue -- Lua doesn't have a continue
		end
		...
		::continue::
	end
	
	return tiles
end
```

now the level can get more interesting
* maybe a chasm is generated, followed by a pillar, creating a rather than tall or harsh obstacle
* almost like someone intentionally does that

think of Mario again: pyramid is also a common thing in it
* and there are some variety to it too

___

# Avoid unnecessary render

In our example, we're actually render the whole world or the whole level, even we don't get to see the parts that are outside of the screen.

For a small example as ours, it's fine. However, for a game like Terraria, which has a world that's thousands and thousands of tiles wide, it'll run into issue.
* In cases like that, we should render only a chunk, only the parts that we can see.
* So maybe only render from the tile above and on the left of the screen to the tile below and on the right of the screen, using just a for loop.
___

# Tile Collision

Since the whole world is tile based, we can convert coordinates to tiles
* just divide coordinates by `TILE_SIZE`, and remember to plus 1
* we can just check in which tile a specific coordinate is, and ask what's the type of that tile
	* is it solid or collidable or not?
* so we don't need to do AABB collision detection against every solid tile, like [[2-2 Collision#breakout-4: collision|how Breakout does]]
	* or at least a collection of tiles
	* saving us computing time
		* O(n) vs O(1)
		* kinda like [[CS50x 5-2 Linked Lists#Win?|arrays versue linked lists]]
* although we still need AABB collision for moving tiles

Remember to shrink the character's collision box
* because if there is a tile wide gap and if the character's collision box is a tile wide, it is likely that it won't fall down into the gap no matter how precisely it stands exactly on the gap
* the collision detection is triggered on both the left and right ground
* so we at least need to shrink it down by one pixel

Back to how the tile based collision detection is done
for example the character is jumping
* assuming the whole level is static, meaning nothing would fly towards the character and collider with it when it's still in the middle is jumping
	* so no moving floating platforms, no moving enemies, and the likes
* the character can only collide with solid tiles that are above it
* so we will check its top-left pixel and top-right pixel, and if either of them is inside of a solid tile, it triggers a collision
	* we can't just check only one side
	* because there could be cases like the top-left corner isn't inside of a tile but the top-right is, and vice versa

```lua
--! file: TileMap.lua
...
function TileMap:pointToTile(x, y)
	if x < 0 or x > self.width * TILE_SIZE or y < 0 or y > self.height * TILE_SIZE then
		-- the point is outside of the map, return here to avoid index error
		return nil
	end
	-- actual code for turning a point to a tile
	return self.tiles[math.floor(y / TILE_SIZE) + 1][math.floor(x / TILESIZE) + 1]
end
...
```

And we don't need to perform collision detection on every corner of the character's collision box
* if it's jumping, just check the top side
* if it's falling or walking, just check the bottom side
* if it's jumping, falling, or walking, check the side which the character is facing

* The Player class has `checkLeftCollisions(dt)` and `checkRightCollisions(dt)`, but not `checkTopCollisions(dt)` and `checkBottomCollisions(dt)`
	* because they are in the the `PlayerJumpState` and `PlayerFallingState` respectively
	* but actually the functions are replaced by a `onCollide` callback

Notice we just check any 2 corners of the character
* that's because our character is no taller than 2 tiles
* if the character is 3 tiles tall or even taller, 8 tiles tall
	* we would need to check for 9 points along the 8 tiles edge, each seperated by `TILE_SIZE`
	* otherwise the character can walk through floating solid tiles if we just checked the 2 corners
___
