# Pokemon

* released in 1997
	* in Japan it's 1996 though
* the first Pokemon game, Red and Blue, additionally Green

* You're a Pokemon trainer
* you go out and explore the world to capture 151 different types of creatures called Pokemon
	* types in the sense of element types
	* eg. Weepinbell is a grass type, Geodude is a rock type
* some types are better than other types
	* sort of a very large rock, paper, scissors relationship

* You have a customized team of these creatures that you had caught and raised and battled
	* to fight other trainers
* You can fight your friends, or trade Pokemon with others
	* you would often share stories back and forth about the different rare creatures that you would have encountered

1. Red and Blue
	* released in 1997 for Gameboy
2. Gold and Silver
	* released a couple of years afterwards for the Gameboy Color
	* new features including breeding, and a day night cycle, etc.
		* became part of the core of the series
3. Ruby and Sapphire
	* for the Gameboy Advance, a significant graphical update
	* same core formula
4. Diamond and Pearl
	* for NDS, which made use of 2 screens
5. Black and White
	* introduced 3D graphics, for the over world
6. X and Y
	* released recently, for the 3DS
7. Alpha Sapphire and Omega Ruby
	* 3DS as well
8. Moon and Sun
	* 3DS

RPG genre with an unique take
* *role playing game*
* [How to Make an RPG](https://howtomakeanrpg.com/)
	* not free though, but very worth-reading
___

# State Stack

before, we used a [[1-3 State Machine#bird-8: State Machine|State Mahcine]]
* so it's like we have only 1 box and 1 socket, and we're always looking at this one socket
	* PlayState, BattleState, StartState, etc., they're all there

now, we're transitioning to the idea of, stacking states
* rather than just render one state, we can render multiple states at a time

| stack | state |
| :-: | :-: |
| 3 | fade-in state |
| 2 | dialogue state |
| 1 | play state |

1. maybe we're in the play state, we're walking around and encountered an NPC and talked to
2. so now we need to do not only the play state things, but also the dialogue state things
	* our previous model, the State Machine, would completely eliminate the play state if we do transition to a second state
	* because there can be only one state being active

* we would render the states sequentially, based on the order that they were popped in
	* from the bottom up
* But, we only really ever need to update one state at a time
	* usually we only need to update whatever is on top
	* because we don't want the character to be able to move around while the dialogue is on, as if the character is talking to someone
		* so only the dialogue state should take input
* Then, when the dialogue finished, we pop it off, and back to updating the play state

> [!info]
> This isn't the only state stack model. Certainly we can do things like updating both dialogue state and play state.
> 
> But that's still using a state stack of sorts.

another use case is for the battle
* we have a play state and we push a battle state on top of that
* we don't see the play state underneath, but as soon as we pop the battle state off, we're right back where we just were in the play state

```lua
--! file:StateStack.lua
StateStack = Class{}

function StateStack:init()
	self.states = {}
end

function StateStack:update(dt)
	self.states[#self.states]:update(dt)
end

function StateStack:processAI(params, dt)
	self.states[#self.states]:processAI(params, dt)
end

function StateStack:render()
	for i, state in ipairs(self.states) do
		state:render()
	end
end

function StateStack:clear()
	self.states = {}
end

function StateStack:push(state)
	table.insert(self.states, state)
	state:enter()
end

function StateStack:pop()
	self.states[#self.states]:exit()
	table.remove(self.states)
end
```

`self.states[#self.states]`
* because what ever state that we pushed last, so the top stacked state, would be the last index in the table
* doesn't have to be the last though, can be the first as well
	* still, being the last would be better
	* for performance sake, `table.remove(1)` vs `table.remove(self.states)`, the former needs to shift all other items in the table
		* not a very big deal though
* so what ever order you can make sense of

`table.remove(self.states)`
* when we call `table.remove()` and pass in only the table, it'll remove the last index
___

## Start State
like Match 3, we'd have a transition going from the start state to the play state

in Match 3, it's a rectangle that filled the entire screen and fade over time, using tween
but recall where the rectangle is stored?
* [[3-1 Tween & Chain#tween-2: `Timer.tween()`|it's the Start State]]
* but thinking about it, Start State isn't really about the transition
	* so actually it'd be better to decouple, Start State and transition

Why? because what if we wanted to make a transition between every single state?
* we need a rectangle and an opacity in every single state
	* the 2 fields being not relevant to the states at all
* since we have state stack now, how about turning transition into its own state?
	* it's no longer a part of any state

```lua
--! file: StartState.lua
function StartState:init()
	-- some start screen stuffs
	...
end

function StartState:update(dt)
	if love.keyboard.wasPressed('enter') or love.keyboard.wasPressed('return') then
		gStateStack:push(FadeInState({
			r = 255, g = 255, b = 255 -- fade to color
		}, 1,       -- duration, how long to fade
		function()  -- when the fade finishes, call this function
			gSounds['intro-music']:stop()
			self.tween:remove()
			
			gStateStack:pop() -- pop itself (StartState) off the stack
			
			gStateStack:push(PlayState())         -- push PlayState
			gStateStack:push(DialogueState("" ..  -- push DialogueState
				"..."
			))
			gStateStack:push(FadeOutState({       -- push FadeOutState
				r = 255, g = 255, b = 255
			}, 1, function() end))
		end))
	end
end
```

`FadeInState(RGB, duration, callback)`
* why RGB instead of RGBA? because A is automatically handled
	* goes from 0 to 255
	* there's a `FadeOutState`, in which A goes from 255 to 0
* since `FadeInState`, by nature, is an asynchronous state
	* it does its behavior over time
	* if we want it to perform something not immediately, but after it finishes, we implement 'something' in a callback and have the state calls the function when it finishes
		* eg. we don't know when the player is going to press the Spacebar on the dialogue state and clear the window
		* but we want the game to enter battle state when the dialogue closed

> [!tip]
> Callback is key to chain asynchronous behavior.
> 
> GUI being one of the things that are very callback-driven, because the nature of it being based on user input. Like when a button is pressed, call the function that we passed into that GUI widget.

* when the `FadeInState` finished, we pop the `StartState`, then push the `PlayState`, then the `DialogueState`, then the `FadeOutState`
	* all at once
	* because only the top layer get updated, we have to lay the foundation and order things in a right order or correct flow
* so keep in mind how the updating and rendering work in our State Stack model

```lua
--! file: main.lua
function love.load()
	...
	gStateStack = StateStack()
	gStateStack:push(StartState())
	...
end
```

recall, previously with `gStateMachine`
* we would give it a list of indexes into anonymous functions, which will the instantiation of the states
* later when we called `change()`, the state machine will index into its list of anonymous function to instantiate the state and set it to `gStateMachine.current`

now we just create a `StateStack`, and push to it a `StartState`
* then we can layer things onto it
* we're not gonna ever get rid of it
	* we can though, but we don't have to
	* especially for the play state, which we pretty much would never pop off from the stack
		* because all the information are there
		* eg. all the character information, all the Pokemon information, inventory, etc.
* then the battle state or other states will just pull information from that
	* then construct a battle as needed

## Fade-in State

```lua
--! file: FadeInState.lua
function FadeInState:init(color, time, onFadeComplete)
	self.r = color.r
	self.g = color.g
	self.b = color.b
	self.opacity = 0
	self.time = time
	
	Timer.tween(self.time, {
		[self] = {opacity = 255}
	})
	:finish(function()
		gStateStack:pop() -- pop itself off the stack
		onFadeComplete()  -- the asynchronous function or the callback parameter
	)
end
```
___

# Tile based movement

similar to what we did with [[5-1 Dungeon and Data#Our Dungeon Code|Zelda]]
* except RPGs usually are tile based to the degree of even movements are tile based
	* eg. Final Fantasy, Pokemon, Dragon Quest
	* player moves 1 tile at a time, and always sticks hard set to a grid
		* can make it looks like as if we have free movement though, by tweening
* it's sort of like a trend, probably not a strictly necessary thing
	* perhaps it's just a symptom of tile based games from the NES and Gameboy era
		* because they're very tile based systems, it's easier to design and implement tile based movements

We can do this by tweening player's sprite position, rather than its position, on it moves
* also we'd want to disable input while the character is walking

```lua
--! file: EntityWalk.lua
function EntityWalkState:attemptMove()
	self.entity:changeAnimation('walk-' .. tostring(self.entity.direction))
	
	local toX, toY = self.entity.mapX, self.entity.mapY
	if self.entity.direction == 'left' then
		toX = toX - 1
	elseif self.entity.direction == 'right' then
		toX = toX + 1
	elseif self.entity.direction == 'up' then
		toY = toY - 1
	else
		toY = toY + 1
	end
	-- so toX and toY are the next mapX and mapY
	
	if toX < 1 or toX > 24 or toY < 1 or toY > 13 then
		-- if it's trying to go outside the boundaries, change it back to idle
		self.entity:changeState('idle')
		self.entity:changeAnimation('idle-' .. tostring(self.entity.direction))
		return
	end
	
	-- execute the move
	self.entity.mapX = toX
	self.entity.mapY = toY
	
	Timer.tween(0.5, {
		[self.entity] = {
			x = (toX - 1) * TILE_SIZE,
			y = (toY - 1) * TILE_SIZE - self.entity.height / 2
			-- make it so that it looks like standing on top of the tile
		}
	}):finish(function()
		-- repeat walking, if player is still pressing the walk buttons
		if love.keyboard.isDown('left') then
			self.entity.direction = 'left'
			self.entity:changeState('walk')
		elseif love.keyboard.isDown('right') then
			...
		...
		else
			self.entity:changeState('idle')
		end
	end)
end
```

Every entity has a x and a y, also a mapX and a mapY
* the regular x and y, are used to draw the sprite at a specific location on the map
* we calculate `x` or `y` by `mapX` or `mapY` times `TILE_SIZE`
___

# Play State

```lua
--! file: PlayState.lua
function PlayState:init()
	self.level = Level() -- contain Entities, Objects, etc.
	
	gSounds['field-music']:setLooping(true)
	gSounds['field-music']:play()
	
	self.dialogueOpened = false
end

function PlayState:update(dt)
	if not self.dialogueOpened and love.keyboard.wasPressed('p') then
		-- heal pokemons
		gSounds['heal']:play()
		-- currentHP, current HP; HP, max HP.
		self.level.player.party.pokemon[1].currentHP = self.level.player.party.pokemon[1].HP
		
		gStateStack:push(DialogueState('Your Pokemon has been healed!'),
		function()
			self.dialogueOpened = false
		end)
	end
	
	self.level:update(dt)
end

function PlayState:render()
	self.level:render()
end
```

> [!tip]
> Keeping track of a current HP and a max HP is essentially how we do stat changes in games. Like health and MP, and all of those things. Depending on whether we're buffed or debuffed, taken damage or not, used spells or not, we manipulate the current value by some amount, and we can always return back to the initial value.

So when ever we pressed `'p'` to heal our pokemon, we opened up a dialogue
* notice we can't move while the dialogue is opened
	* because `DialogueState` is the top state and `PlayState` is the second top
* notice also we passed in a callback to `DialogueState`, but when does it get called?
	* when the user closes the dialog box

```lua
--! file: DialogueState.lua
DialogueState = Class{__includes = BaseState}

function DialogueState:init(text, callback)
	self.textbox = Textbox(6, 6, VIRTUAL_WIDTH - 12, 64, text, gFonts['small'])
	self.callback = callback or function() end
end

function DialogueState:update(dt)
	self.textbox:update(dt)
	
	if self.textbox:isClosed() then
		-- if the textbox is closed
		self.callback()
		gStateStack:pop()
	end
end

function DialogueState:render()
	self.textbox:render()
end
```
___

## Level

```lua
--! file: Level.lua
function Level:init()
	self.tileWidth = 50
	self.tileHeight = 50
	
	-- the map has a base floor tile and some tall grass tiles
	self.baseLayer = TileMap(self.tileWidth, self.tileHeight)
	self.grassLayer = TileMap(self.tileWidth, self.tileHeight)
	-- used when characters stand on the grass
	self.halfGrassLayer = TileMap(self.tileWidth, self.tileHeight)
	
	-- set-up self.player
	...
end
```

In our case, player has a 10% chance encountering Pokemons whenever he starts walking, so whenever player pressed the button and it happens to be on grass
* sometimes games make it more likely to encounter events, the more steps we've taken
* some games will just be completely random, but this is less robust

```lua
--! PlayerWalkState.lua
function PlayerWalkState:enter()
	self:checkForEncounter()
	
	if not self.encounterFound then
		self:attemptMove()
	end
end

function PlayerWalkState:checkForEncounter()
	local x, y = self.entity.ampX, self.entity.mapY
	
	if self.level.grassLayer.tiles[y][x].id == TILE_IDS['tall-grass'] and math.random(10) == 1 then
		self.entity:changeState('idle')
		
		gSounds['field-music']:pause() -- rather than stop()
		gSounds['battle-music']:play()
		
		gStateStack:push(
			-- push FadeInState on top of PlayState
			FadeInState({
				r = 255, g = 255, b = 255
			}, 1,
			function()
				-- when FadeInState finished, it pop itself off the stack
				
				-- information of our Pokemon party is in self.entity
				gStateStack:push(BattleState(self.entity))
				gStateStack:push(FadeOutState({
					r = 255, g = 255, b = 255
				}, 1,
				function()
					-- when FadeOutState finished, it pop itself off
					-- results in a BattleState on top of a PlayState
				end))
			end)
		)
		self.encounterFound = true
	else
		self.encounterFound = false
	end
end
```

Notice the entities are still using just a regular state machine, not a state stack
* it's not necessary in our game, but sometimes we may find it useful to use state stack for entities
___
