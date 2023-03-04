# Super Mario Bros.

* came out in 1985
* probably the most famous game of all time

* revolute the gaming industry
	* took the gaming crash of the late '70s, early '80s, and brought games to the forefront of people's consciousness
* one of the titles that made Nintendo basically the dominator of the video games industry
	* in the '80s and '90s
	* even untill todays

* inspired and influenced many good quality platformers
___

# Tilemaps

* it effectively is a 2D array, or a table, of numbers, IDs
* the ID defines the apperance or behavior of different tiles
	* maybe some numbers represent that the tile is solid, collidable

eg.
* so the world can have empty tiles that don't collide with the player, maybe it's the background, the level decorations, etc., and tiles that are collidable like grounds, walls, etc.
* maybe we would have animated tiles as well
___

# tiles-0: static tiles

firstly we need to have our tile sprites ready
in our case it's just a sprite with two tiles
* one (ID 1) is a single tiny block of bricks
* one (ID 2) is complete transparent, it's the empty block

```lua
TILE_SIZE = 16

-- the tile IDs
SKY = 2
GROUND = 1

function love.load()
	math.randomseed(os.time())
	
	tiles = {} -- represents the level, or the game world
	
	tilesheet = love.graphics.newImage('tiles.png')
	quads = GenerateQuads(tilesheet, TILE_SIZE, TILE_SIZE)
	
	mapWidth = 20  -- level has 20 blocks horizontally
	mapHeight = 20 -- and 20 blocks vertically
	
	backgroundR = math.random(255)
	backgroundG = math.random(255)
	backgroundB = math.random(255)
	
	for y = 1, mapHeight do
		table.insert(tiles, {})
		for x = 1, mapWidth do
			table.insert(tiles[y], {
				-- any where higher than the fifth block from the top,
				-- it's the sky, and otherwise the ground
				id = y < 5 and SKY or GROUND
			})
		end
	end
	
	-- other set-ups
	...
end

function love.draw()
	push:start()
		love.graphics.clear(backgroundR, backgroundG, backgroundB, 255)
		
		for y = 1, mapHeight do
			for x = 1, mapWidth do
				local tile = tiles[y][x]
				love.graphics.draw(tilesheet, quads[tile.id], (x - 1) * TILE_SIZE, (y - 1) * TILE_SIZE)
			end
		end
	push:finish()
end
```
___

# tiles-1: scrolling tiles

most of the times the world and the tiles would scroll
* maybe it's an airplane that's going through the level that's scrolling automatically
* maybe it's like Mario and the camera is always following the avatar, so the scrolling is just relative to the character's position

`love.graphics.translate(x, y)`
* shifts the corrdinate system by x, y specified
	* whenever we draw something, it's automatically shifted
* useful for simulating camera behavior

if we maintain a reference to where the character is, we can shift where everything gets drawn accordingly. and this is how camera in 2D games work.
* but it is not. we're shifting the coordinate system but not the camera.

```lua
...
function love.update(dt)
	if love.keyboard.isDown('left') then
		cameraScroll = cameraScroll - CAMERA_SCROLL_SPEED * dt
	elseif love.keyboard.isDown('right') then
		cameraScroll = cameraScroll + CAMERA_SCROLL_SPEED * dt
	end
end

function love.draw()
	push:start()
		love.graphics.translate(-math.floor(cameraScroll), 0)
		... -- draw the tiles
	push:finish()
end
```

`love.graphics.translate(-math.floor(cameraScroll), 0)`
* why we need to negate cameraScroll?
	* if we want to see the left end of the level, the world should go right, vice versa.
* then why `math.floor(cameraScroll)`?
	* first of all we're using push to render virtual resolution
		* the game is drawn on a virtual canvas that's been magnified or condensed
	* then if we offset or translate things by a fractional amount, we'd get *artifacting*
		* which is a fancy term for weird blur and stuff like that
* so scaled virtual canvas + `love.graphics.translate()`, remember to use `math.floor()`

make sure to do the translation before drawing the stuffs
* because every draw after the translation gets affected by the coordinate system change
* every draw before doesn't


### camera in other tools

a lot of 2D game engines will have a camera object
* so they encapsulate the whole coordinate system thing
* Löve 2D doesn't have a camera, because it's kinda a lower level game framework
	* other more robust solutions like Unity or Phaser, will just have a camera object
___

# character-0: stationary hero

the sprite contains several animations of the hero
* idle, walk, hurted, climb, etc.
* in our case all frames are arranged in a row

for now we just want to make a stationary hero
* so we just need the first frame from the sprite
* and it's just `love.graphics.draw()`

```lua
...
characterSheet = love.graphics.newImage('character.png')
characterQuads = GenerateQuads(characterSheet, CHARACTER_WIDTH, CHARACTER_HEIGHT)

characterX = VIRTUAL_WIDTH / 2 - (CHARACTER_WIDTH / 2) -- center of the screen
characterY = ((7 - 1) * TILE_SIZE) - CHARACTER_HEIGHT
...
function love.draw()
	...
	-- we just need the first frame
	love.graphics.draw(characterSheet, characterQuads[1], characterX, characterY)
end
```

`characterY = ((7 - 1) * TILE_SIZE) - CHARACTER_HEIGHT`
* `(7 - 1)` because tiles are 1 indexed but coordinate is 0 indexed
* `- CHARACTER_HEIGHT` so the character is right above the tile instead of beneath
___

# character-1: moving hero

it's easy
```lua
...
function love.update(dt)
	if love.keyboard.isDown('left') then
		characterX = characterX - CHARACTER_MOVE_SPEED * dt
	elseif love.keyboard.isDown('right') then
		characterY = characterY - CHARACTER_MOVE_SPEED * dt
	end
end
...
```

but what are missing?
1. the camera should track and follow the character
2. the character should play a walking animation ONLY when it's moving
___

# character-2: tracked hero

```lua
...
function love.update(dt)
	... -- move player
	cameraScroll = characterX - (VIRTUAL_WIDTH / 2) + (CHARACTER_WIDTH / 2)
end

function love.draw()
	push:start()
		love.graphics.translate(-math.floor(camaeraScroll), 0)
		... -- draw stuffs
	push:finish()
end
```

`cameraScroll = characterX - (VIRTUAL_WIDTH / 2) + (CHARACTER_WIDTH / 2)`
* `cameraScroll = characterX` would make the character always at the left edge of the screen.
* Then we offset the character's position to where we want it to be on the screen

if we want to track the character on the y-axis, it's the same thing
___

# character-3: animated hero

first of all we now have a `Animation` class
* we pass in the series of frames that we want to draw for an animation, and set an interval, which defines the speed of the animation
* how it works is just a basic timer: if a certain time passed, render the next frame
	* be careful that we need to [[3-1 Tween & Chain#timer-0: basic timer|do modulus so that the timer is accurate]]
* and if the animation just has 1 frame then we don't need the timer

here is how the class is used
```lua
--! file: main.lua
...
idleAnimation = Animation {
	frames = {1},
	interval = 1 -- it's doesn't matter
	-- but if we later changed the animation, we won't forget to set the interval
}
movingAnimation = Animation {
	frames = {10, 11},
	interval = 0.2
}
currentAnimation = idleAnimation
direction = 'right'
...
function love.draw()
	...
	love.graphics.draw(characterSheet,
		characterQuads[currentAnimation:getCurrentFrame()],
		-- we shifted where the character is drawn by half the size of the sprite
		math.floor(characterX) + CHARACTER_WIDTH / 2,  -- x position
		math.floor(characterY) + CHARACTER_HEIGHT / 2, -- y position
		0, direction == 'left' and -1 or 1, 1,  -- rotation, x scale, y scale
		CHARACTER_WIDTH / 2, CHARACTER_HEIGHT / 2)  -- x, y offsets on the sprite
end
```

when we do rotate or scale, it's performed on the origin
* if we flip the character on its origin, that is scale to -1, we will see the character's is drawn on the left of the origin, instead of staying where it is, staying on the right and below the origin
* that' why we have all these shifting things
	* and all these are just to set the origin to the sprite's center

if we just do the x, y offsets but not shifting the x, y positions
* the sprite is drawn on `characterX` and `characterY`, which in this case are meant to be the top-left corner of the character
* so the character would be standing mid air (half the size of the sprite)
___

# character-4: jumping hero

how many do our character now have?
* idle state, walking state, jumping state
* but notice, we also gonna need a falling state

why seperating jumping state and falling state is a important thing? Think of Mario:
* when he is jumping, he can hit blocks; when he is falling, he can destroy enemies; likewise,
* when he is jumping, he can't attack the enemy; when he is falling, he can't destroy the block.

it's something to pay attention to even it does seem a lot like it's one jumping state
* due to they share the same animation
* the behaviors of jumping and falling are quite different

```lua
...
JUMP_VELOCITY = -200
GRAVITY = 7
...
function love.keypressed(key)
	-- check characterDY to prevent jumping infinitely
	if key == 'space' and characterDY == 0 then
		characterDY = JUMP_VELOCITY
		currentAnimation = jumpAnimation
	end
end

function love.update(dt)
	characterDY = characterDY + GRAVITY
	characterY = characterY + characterDY * dt
	
	-- we didn't introduce collision yet
	-- here we're just preventing the character from going below the ground
	if characterY > ((7 - 1) * TILE_SIZE) - CHARACTER_HEIGHT then
		characterY = ((7 - 1) * TILE_SIZE) - CHARACTER_HEIGHT
		characterDY = 0
	end
	...
end
```
___
