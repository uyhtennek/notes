# Pokemon

Pokemon Party is just a container class
* even a fully fleshed game doesn't need anything much more complicated than this

```lua
--! Party.lua
Party = Class{}

function Party:init(def)
	self.pokemon = def.pokemon
end

function Party:update(dt)
	
end

function Party:render()
	
end
```

Pokemon class is similar in spirit,
it's effectively a container of stats
* there are a few other funtionalities as well though
* a lot of RPGs are like this as well
	* mostly just numbers
	* comparing numbers, and then adding a roll of the dice
		* eg. Dungeons & Dragons

```lua
--! file: Pokemon.lua
function Pokemon:init(def, level)
	self.name = def.name
	
	self.battleSpriteFront = def.battleSpriteFront
	self.battleSpriteBack = def.battleSpriteBack
	
	-- stats for a level 0 Pokemon
	self.baseHP = def.baseHP
	self.baseAttack = def.baseAttack
	self.baseDefense = def.baseDefense
	self.baseSpeed = def.baseSpeed
	
	-- IV for individual value, sort of like the DNA of the Pokemon
	self.HPIV = def.HPIV
	self.attackIV = def.attackIV
	self.defenseIV = def.defenseIV
	self.speedIV = def.speedIV
	
	-- the actual stats (max stats)
	self.HP = self.baseHP
	self.attack = self.baseAttack
	self.defense = self.baseDefense
	self.speed = self.baseSpeed
	
	self.level = level
	self.currentExp = 0
	self.expToLevel = self.level * self.level * 5 * 0.75
	
	self:calculateStats()
	
	-- in our game, only HP can change during battle
	self.currentHP = self.HP
end
...
function Pokemon:statsLevelUp()
	-- when a Pokemon levels up
	local HPIncrease = 0
	-- roll 3 dice for HP incrase
	for j = 1,3 do
		if math.random(6) <= self.HPIV then
			self.HP = self.HP + 1
			HPIncrease = HPIncrease + 1
		end
	end
	-- roll 3 dice for attack, defense, speed
	...
	return HPIncrease, attackIncrease, defenseIncrease, speedIncrease
end

function Pokemon:levelup()
	self.level = self.level + 1
	self.expToLevel = self.level * self.level * 5 * 0.75
	
	return self:statsLevelUp()
end
```

*Individual value*, IV
* each Pokemon has an IV, so that 2 Pokemons of the same species, can have different growths
* we roll 3 dice everytime the Pokemon level up
	* the dice has 6-faces but IV only has 1-5
	* if the dice is higher than IV, the Pokemon gets a stat increase
* basically the higher the IV is, the stronger the Pokemon is likely to be
* not every game and the Pokemon game doesn't necessarily implemented this way though

```lua
--! file: pokemon_defs.lua
POKEMON_IDS = {
	'aardart', 'agnite', 'anoleaf', 'bamboon', 'cariwing'
}

POKEMON_DEFS = {
	['aardart'] = {
		name = 'Aardart',
		battleSpriteFront = 'aardart-front',
		battleSpriteBack = 'aardart-back',
		baseHP = 14,
		baseAttack = 9,
		baseDefense = 5,
		baseSpeed = 6,
		HPIV = 3,
		attackIV = 4,
		defenseIV = 2,
		speedIV = 3
	},
	...
}
```

in an ideal world, there'd be moves for each Pokemon as well
* but we'd model moves as data as well

It's just good game design
* making source code and debugging source all day long are not easy and not some desired things to do anyway
___

## Shader

Lastly there comes the Battle Sprite for the Pokemons
* when a Pokemon attacks, the sprite blinks white
* when a Pokemon is being attacked, the sprite flashes

Recall in Zelda we have this `invulunerable` flag for the Player, when it took damage
* while the flag is raised, the Player sprite keep flashing
* we were just jumping between drawing the sprite in full opacity and zero opacity

But, we can't do that with the sprite that's blinking white
* because we can't really make a sprite goes all white just by raising a 'white' flag
* that's something we actually need to use a shader for

Shaders are pretty complex and arcane at first
* but they are just a little program that runs on our graphics card
* it can look at every pixel that we're drawing, and produce a new value for the pixels
	* depending how we implemented the shader
* take a look at the website Shadertoy

```lua
--! file: BattleSprite.lua
function BattleSprite:init(texture, x, y)
	self.texture = texture
	self.x = x
	self.y = y
	self.opacity = 255
	self.blinking = false
	
	-- https://love2d.org/forums/viewtopic.php?t=79617
	-- it turns all the pixels of a sprite completely white
	self.whiteShader = love.graphics.newShader[[
		extern float WhiteFactor;
		
		vec4 effect(vec4 vcolor, Image tex, vec2 texcoord, vec2 pixcoord)
		{
			vec4 outputcolor = Texel(tex, texcoord) * vcolor;
			outputcolor.rgb += vec3(WhiteFactor);
			return outputcolor;
		}
	]]
	-- Data structures for shaders are based on floats that are from 0 to 1
	-- if WhiteFactor is 1, vec3(1) = (1, 1, 1) = pure white
end
...
function BattleSprite:render()
	love.graphics.setColor(255, 255, 255, self.opacity)
	
	love.graphics.setShader(self.whiteShader)
	-- send the value 1 or 0, to the shader's 'WhiteFactor' field
	self.whiteShader:send('WhiteFactor', self.blinking and 1 or 0)
	
	love.graphics.draw(gTextures[self.texture], self.x, self.y)
	
	-- reset shader
	love.graphics.setShader()
end
```

Again, this is not the only solution to get things blinking white
* but arguably the simplest way
___

# Opponent
Every opponent has a party, that's it.

```lua
--! file: Opponent.lua
Opponent = Class{}

function Opponent:init(def)
	self.party = def.party
end

function Opponent:takeTurn()
	
end
```

In a fully fleshed game though, you may also want
* a trainer sprite
* a message that it says
* a gold value that it'd give you when you defeated it
* ....
___

# Battle State

There are quite a number of steps involved in a battle
1. when we press Fight, it kicks off an initial callback
2. Dialog says "A attcked B"
3. the attacker blinks white
4. the one being attacked flashes
5. the health bar of the one begin attacked drops
6. Repeat the steps, but for the other side

* we also need to check if somebody dies
1. if we die, the screen fades black and we go back to the Play State
2. if the opponent dies
	1. the pokemon falls off the screen
	2. push a Battle Message State onto the screen
	3. push a dialogue that tells how many expreience points we earned
	4. the EXP bar tweens
		1. if we leveled up, play the right sounds and music
		2. push a NewMenuState, which tells the player by how much the Pokemon grows
			* by how much the stats increase
		3. when Player pressed Enter, pop the state off
	5. push a Fade In State
	6. pop every states out until there is the PlayState left
	7. push a Fade Out State

```lua
--! file: BattleState.lua
function BattleState:init(player)
	self.player = player
	self.bottomPanel = Panel(0, VIRTUAL_HEIGHT - 64, VIRTUAL_WIDTH, 64)
	
	-- because when we pushed the BattleState, we also pushed a FadeOutState
	-- and we want the Pokemons to slide in only after the fade out
	self.battleStarted = false
	
	self.opponent = Opponent {
		party = Party {
			pokemon = {
				Pokemon(Pokemon.getRandomDef(), math.random(2, 6))
			}
		}
	}
	
	self.playerSprite = BattleSprite(...)
	self.opponentSprite = BattleSprite(...)
	
	-- a green health bar
	self.playerHealthBar = ProgressBar {
		x = VIRTUAL_WIDTH - 160,
		y = VIRTUAL_HEIGHT - 80,
		width = 152,
		height = 6,
		color = {r = 189, g = 32, b = 32},
		value = self.player.party.pokmeon[1].currentHP,
		max = self.player.party.pokemon[1].HP
	}
	
	self.opponentHealthBar = ProgressBar {
		...
	}
	
	self.renderHealthBars = false
	
	-- the circle ellipses where the Pokemons stand on
	-- just a little graphical detail
	self.playerCircleX = -68
	self.opponentCircleX = VIRTUAL_WIDTH + 32
	
	-- references to the fighting pokemons
	self.playerPokemon = self.player.party.pokemon[1]
	self.opponentPokemon = self.opponent.party.pokemon[1]
end

-- state enter and state exit
...

function BattleState:update(dt)
	if not self.battleStarted then
		self:triggerSlideIn() -- slide in the 2 pokemons
	end
end

-- render
...

function BattleState:triggerSlideIn()
	self.battleStarted = true
	
	Timer.tween(1, {
		[self.playerSprite] = {x = 32},
		[self.opponentSprite] = {x = VIRTUAL_WIDTH - 96},
		[self] = {playerCircleX = 66, opponentCircleX = VIRTUAL_WIDTH - 70}
	})
	:finish(function()
		self.triggerStartingDialogue()
		self.renderHealthBars = true
	end)
end

function BattleState:triggerStartingDialogue()
	gStateStack:push(BattleMessageState('A wild ' .. tostring(self.opponent.party.pokemon[1].name .. ' appeared!'),
	-- callback
	function()
		gStateStack:push(BattleMessageState('Go, ' .. tostring(self.player.party.pokemon[1].name .. '!'),
		-- callback
		function()
			gStateStack:push(BattleMenuState(self))
		end))
	end))
end
```

Exp bar is just the same as HP bar
* both are Progress Bar
* one is green and one is blue
___

# Battle Menu State

```lua
--! file: BattleMenuState.lua
function BattleMenuState:init()
	...
	-- recall Menu.selection is where we store the items
	self.battleMenu = Menu {
		x = VIRTUAL_WIDTH - 64,
		y = VIRTUAL_HEIGHT - 64,
		width = 64,
		height = 64,
		items = {
			{
				text = 'Fight',
				onSelect = function()
					-- callback that will be executed when we selected this item
					gStateStack:pop()
					-- TakeTurnState is the state where Pokemons fight each other
					gStateStack:push(TakeTurnState(self.battleState))
				end
			},
			{
				text = 'Run',
				onSelect = function()
					gSounds['run']:play()
					
					gStateStack:pop()
					
					-- in our example we can 100% flee
					gStateStack:push(BattleMessageState('You successfully flee.'),
						function() end, false)
						-- the false in here, means the player can't input in the state
						-- so player can't press Enter to quit the BattleMessageState
					
					-- so remember to get rid of the state somehow somewhere
					-- we used a timer
					Timer.after(0.5, function()
						gStateStack:push(FadeInState({
							r = 255, g = 255, b = 255
						}, 1,
						function()
							gSounds['field-music']:play()
							
							-- pop BattleMessageState
							gStateStack:pop()
							-- pop BattleState
							gStateStack:pop()
							
							gStateStack:push(FadeOutState({
								r = 255, g = 255, b = 255
							}, 1, function()
								-- do nothing after fade out ends
							end))
						end))
					end)
				end
			}
		}
	}
end
```

> [!tip]
> Why call it `TakeTurnState` rather than just `FightState`?
> 
> It's a better name because in this state Pokemons can fight, run, use items, throw a Poke ball, etc. Not necessarily only fighting each other. Therefore the name should tell clearly that this is a general purpose state, rather than this is a state where Pokemons fights.
> 
> In our example though, they can only fight. No other interesting options.

___

# Take Turn State

```lua
--! file: TakeTurnState.lua
function TakeTurnState:init(battleState)
	self.battleState = battleState
	self.playerPokemon = self.battleState.player.party.pokemon[1]
	self.opponentPokemon = self.battleState.opponent.party.pokemon[1]
	
	self.playerSprite = self.battleState.playerSprite
	self.opponentSprite = self.battleState.opponentSprite
	
	if self.playerPokmeon.speed > self.opponentPokemon.speed then
		-- if our pokemon is the faster one
		self.firstPokemon = self.playerPokemon
		self.secondPokemon = self.opponentPokemon
		self.firstSprite = self.playerSprite
		self.secondSprite = self.opponentSprite
		self.firstBar = self.battleState.playerHealthBar
		self.secondBar = self.battleState.opponentHealthBar
	else
		-- if the opponent's pokemon is the faster one
		...
	end
end

function TakeTurnState:enter(params)
	self:attack(
		self.firstPokemon, self.secondPokemon,
		self.firstSprite, self.secondSprite,
		self.firstBar, self.secondBar,
		-- called as soon as the attack finishes
		function()
			-- the attack would push a BattleMessageState, so we need to pop that
			gStateStack:pop()
			
			if self:checkDeaths() then
				-- if any pokemon dies, the battle is over
				gStateStack:pop()
				return
			end
			
			-- do another attack, but this time the secondPokemon is the attacker
			self:attack(
				self.secondPokemon, self.firstPokemon,
				...,
				function()
					gStateStack:pop()
					
					if self:checkDeaths() then
						...
					end
					
					-- pop TakeTurnState
					gStateStack:pop()
					gStateStack:push(BattleMenuState(self.battleState))
				end
			)
		end
	)
end

function TakeTurnState:attack(attacker, defender, attackerSprite, defenderSprite, attackerBar, defenderBar)
	gStateStack:push(BattleMessageState(attacker.name .. 'attacks' .. defender.name,
		function() end, false)) -- we don't take input in the state
	
	-- play the attack animation
	Timer.after(0.5, function()
		gSounds['powerup']:stop()
		gSounds['powerup']:play()
		
		Timer.every(0.1, function()
			-- true becomes false, false becomes true
			attackerSprite.blinking = not attackerSprite.blinking
		end)
		:limit(6) -- only execute the tween 6 times, so only blink 3 times
		:finish(function()
			-- play the hurt animation
			
			gSounds['hit']:stop()
			gSounds['hit']:play()
			
			Timer.every(0.1, function()
				defenderSprite.opacity = defenderSprite.opacity == 64 and 255 or 64
			end)
			:limit(6)
			:finish(function()
				-- calculate damage
				local dmg = math.max(1, attacker.attack - defender.defense)
				
				-- reflect the damage in the HP bar
				Timer.tween(0.5, {
					-- notice the bar is from BattleState, but we're in TakeTurnState
					[defenderBar] = {value = defender.currentHP - dmg}
				})
				:finish(function()
					-- the Progress Bar value is independent from the Pokemon
					defender.currentHP = defender.currentHP - dmg
					onEnd()
				end)
			end)
		end)
	end)
end

function TakeTurnState:checkDeaths()
	if self.playerPokemon.currentHP <= 0 then
		-- we lose
		self:faint()
		return true
	elseif self.opponentPokemon.currentHP <= 0 then
		-- we win
		self:victory()
		return true
	end
	-- neither, the battle continues
	return false
end

function TakeTurnState:faint()
	Timer.tween(0.2, { ... })
	:finish(function()
		gStateStack:push(BattleMessageState('You fainted!',
		function()
			gStateStack:push(FadeInState({
				-- we fade to black, as opposed to fade to white as victory or flee does
				-- to differentiate we're fainting
				r = 0, g = 0, b = 0
			}, 1,
			function()
				-- restore pokmeon to full health
				self.playerPokemon.currentHP = self.playerPokemon.HP
				
				-- resume field music
				...
				
				gStateStack:pop()
				gStateStack:push(FadeOutState({
					r = 0, g = 0, b = 0
				}, 1, function()
					-- right when we resume to the PlayState, push a DialogueState
					gStateStack:push(DialogueState('Your Pokemon has been fully restored; try again!'))
				end))
			end))
		end))
	end)
end

function TakeTurnState:victory()
	Timer.tween(0.2, {
		-- opponent pokemon falls off screen
		[self.opponentSprite] = {y = VIRTUAL_HEIGHT}
	})
	:finish(function()
		-- play victory music
		...
		
		gStateStack:push(BattleMessageState('Vicotry!',
		function()
			-- calculate exp earned
			local exp = ...
			
			gStateStack:push(BattleMessageState('You earned ' .. tostring(exp) .. ' experience points.'), function() end, false)
			
			Timer.after(1.5, function()
				gSounds['exp']:play()
				
				Timer.tween(0.5, {
					-- tween exp bar, math.min() to stop it from going over the bar
					[self.battleState.playerExpBar] = {value = math.min(self.playerPokemon.currentExp + exp, self.playerPokemon.expToLevel)}
				})
				:finish(function()
					gStateStack:pop()
					
					self.playerPokemon.currentExp = self.playerPokemon.currentExp + exp
					
					if self.playerPokemon.currentExp > self.playerPokemon.expToLevel then
						-- pokemon leveled up
						gSounds['levelup']:play()
						
						-- carry over Exp
						self.playerPokemon.currentExp = self.playerPokemon.currentExp - self.playerPokemon.expToLevel
						self.playerPokemon:levelup()
						
						gStateStack:push(BattleMessageState('Congratulations! Level Up!',
						function()
							-- just a helper function to push a white FadeOutState
							self:fadeOutWhite()
						end))
					else
						self:fadeOutWhite()
					end
				end)
			end)
		end))
	end)
end

function TakeTurnState:fadeOutWhite()
	-- fade in
	gStateStack:push(FadeInState({ ... }, 1, function()
		-- resume field music
		...
		
		-- pop BattleState
		gStateStack:pop()
		gStateStack:push(FadeOutState({ ... }, 1, function() end))
	end))
end
```

We could keep a reference of the Pokemon's sprite and health bar in the Pokemon class
* it can shorten the code a bit
* however it's a bit of code smell there
	* because a Pokemon doesn't necessarily have a sprite and a health bar at all time
* therefore we would rather seperating them
	* than putting unnecessary code in the Pokemon class
	* although it makes our `TakeTurnState` code a bit clumsy

notice we're using a tween to change the value of `defenderBar`, which is from the BattleState, but we're currently in the TakeTurnState
* as long as we have reference we can manipulate other states
* regardless if the state is the top state in the stack or not
___

# Missing Features

* Deatiled Level-Up Screen
* Monster Catching
	* the main appeal
	* also a Field Menu For Browsing Pokemon
* Item Inventory
* Different Abilities and Moves
	* also elemental attributes
	* better to be data
* Other Trainers to Fight
* Monster Evolution
	* another main appeal
* Towns, Routes, etc.
* Breeding
	* take 2 pokemons to breed an egg, which has a chance to have really good stats or really rare pokemons
* Day/Night Cycle
	* maybe different Pokemons come out in different time in a day
___
