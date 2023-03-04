# breakout-9: progression

a level is just represented by a number
* where or when do we increment the number though?
* when we cleared all the bricks
	* all the bricks got their `inPlay` flag set to `false`

```lua
--! file: src/states/StartState.lua
...
function StartState:update(dt)
	...
	-- if pressed 'enter' or 'return'
	if ... then
		gSounds['confirm']:play()
		if highlighted == 1 then
			gStateMachine:change('serve', {
				paddle = Paddle(1),
				bricks = LevelMaker.createMap(1),
				health = 3,
				score = 0,
				level = 1 -- the very beginning of the game, the first level
			})
		end
	end
	...
end
...
```

```lua
--! file: src/states/PlayState.lua
...
function PlayState:update(dt)
	...
	for k, brick in pairs(self.bricks) do
		if brick.inPlay and self.ball:collides(brick) then
			self.score = self.score + (brick.tier * 200 + brick.color * 25)
			
			brick:hit()
			
			if self:checkVictory() then
				gSounds['victory']:play()
				
				gStateMachine:change('victory', {
					level = self.level,
					paddle = self.paddle,
					health = self.health,
					score = self.score,
					ball = self.ball
				})
			end
		end
	end
	...
end
...
function PlayState:checkVictory()
	for k, brick in pairs(self.bricks) do
		if brick.inPlay then
			return false
		end
	end
	
	return true
end
```

```lua
--! file: src/states/VictoryState.lua
VictoryState = Class{__includes = BaseState}
...
function VictoryState:update(dt)
	...
	-- if pressed 'enter' or 'return'
	if ... then
		gStateMachine:change('serve', {
			-- the progression happens here
			level = self.level + 1,
			bricks = LevelMaker.createMap(self.level + 1),
			paddle = self.paddle,
			health = self.health,
			score = self.score
		})
	end
end

function VictoryState:render()
	self.paddle:render()
	self.ball:render()
	
	renderHealth(self.health)
	renderScore(self.score)
	
	love.graphics.setFont(gFonts['large'])
	love.graphics.printf("Level " .. tostring(self.level) .. " Completed!",
		0, VIRTUAL_HEIGHT / 4, VIRTUAL_WIDTH, 'center')
	
	love.graphics.setFont(gFonts['medium'])
	love.graphics.rpintf('Press Enter to serve!', 0, VIRTUAL_HEIGHT,
		VIRTUAL_WIDTH, 'center')
end
```
___

# breakout-10: high score
introducing, `love.filesystem`

> [!info]
> LÖVE automatically gives a save directory. It's pretty much hard coded, and it's the only folder that LÖVE has read and write access to. We're not really supposed to not use that directory, although there are a few exceptions.
> 
> It's in `AppData\Roaming\LOVE` on Windows or `Application/Support/LOVE/` on Mac.

`love.filesystem.setIdentity(identity)`
* set the active subfolder in the default LÖVE save directory for reading and writing files to

`love.filesystem.exists(path)`
* check if a file exists in the save directory

`love.filesystem.write(path, data)`
* writes data, as a string, to a file at `path`

`love.filesystem.lines(path)`
* an iterator, which allows us to look over the string lines in a file at `path`

> [!tips]
> Not only Windows and Mac, LÖVE file system also supports Android, officially. iOS though, there is no official documentation, but it should work the same.
> LÖVE would create a dedicated save directory for itself, along with the LÖVE app.

The way that the save file is structured, is as simple as
* this is not the only way though, but it get the job done
```txt
Name 1
Score 1
Name 2
Score 2
Name 3
Score 3
...
```

```lua
--! file: main.lua
...
function loadHightScores()
	love.filesystem.setIdentity('breakout')
	
	if not love.filesystem.exists('breakout.lst') then
		-- if the file doesn't exist
		local scores = ''
		for i = 10, 1, -1 do
			scores = scores .. 'AAA\n'
			scores = scores .. tostring(i * 1000) .. '\n'
		end
		love.filesystem.write('breakout.lst', scores)
	else
		-- if the file does exist
		local name = true
		local currentName = nil
		local counter = 1
		
		local scores = {}
		
		for i = 1, 10 do
			scores[i] = {
				name = nil,
				score = nil
			}
		end
		
		for line in love.filesystem.lines('breakout.lst') do
			if name then -- name line
				-- truncate the name to just 3 letters
				scores[counter].name = string.sub(line, 1, 3)
			else         -- score line
				scores[counter].score = tonumber(line)
				counter = counter + 1
			end
			name = not name
		end
	end
	
	return scores
end
```
___

## breakout-11: score entry

we'll do pretty arcade style name entry here
* eg. AAA, COT, KEN, SIX, etc.
* it's a good place to use ASCII

```lua
--! file: src/states/EnterHighScoreState
EnterHighScoreState = Class{__includes = BaseState}

local chars = {
	[1] = 65, -- 65 is the ASCII value for letter 'A'
	[2] = 65,
	[3] = 65
}

local highlightedChar = 1

function EnterHighScoreState:enter(params)
	self.highScores = params.highScores
	self.score = params.score
	self.scoreIndex = params.scoreIndex
end

function EnterHighScoreState:update(dt)
	if love.keyboard.wasPressed('enter') or love.keyboard.wasPressed('return')
		local name = string.char(chars[1]) .. string.char(char[2]) .. string.char(char[3])
		-- we only need to change the scores that are lower than self.score
		-- i.e. higher than self.scoreIndex
		for i = 10, self.scoreIndex, -1 do
			-- shift all the scores down
			self.highScore[i + 1] = {
				name = self.highScores[i].name,
				score = self.highScores[i].score
			}
		end
		
		self.highScores[self.scoreIndex].name = name
		self.highScores[self.scoreIndex].score = self.score
		
		-- write scores to file
		local scoresStr = ''
		
		for i = 1, 10 do
			scoresStr = scoresStr .. self.highScores[i].name .. '\n'
			scoresStr = scoresStr .. tostring(self.highScores[i].score) .. '\n'
		end
		
		love.filesystem.write('breakout.lst', scoresStr)
		
		-- change state
		gStateMachine:change('high-scores', {
			highScores = self.highScores
		})
	end
	...
	-- scroll through characters
	if love.keyboard.wasPressed('up') then
		chars[highlightedChar] = chars[highlightedChar] + 1
		if chars[highlightedChar] > 90 then
			chars[highlightedChar] = 65
		end
	elseif love.keyboard.wasPressed('down') then
		chars[highlightedChar] = chars[highlightedChar] - 1
		if chars[highlightedChar] < 65 then
			chars[highlightedChar] = 90
		end
	end
end

function EnterHighScoreState:render()
	...
end
```
___

# breakout-12: paddle select

```lua
--! file: src/states/PaddleSelectState.lua
...
function PaddleSelectState:init()
	self.currentPaddle = 1
end

function PaddleSelectState:update(dt)
	-- press left or right to select paddles
	if love.keyboard.wasPressed('left') then
		if self.currentPaddle == 1 then
			gSounds['no-select']:play()
		else
			gSounds['select']:play()
			self.currentPaddle = self.currentPaddle - 1
		end
	elseif love.keyboard.wasPress('right') then
		if self.currentPaddle == 4 then
			gSounds['no-select']:play()
		else
			gSounds['select']:play()
			self.currentPaddle = self.currentPaddle + 1
		end
	end
	
	-- press 'enter' to enter server state
	if love.keyboard.wasPressed('return') or love.keyboard.wasPressed('enter')
		gSounds['confirm']:play()
		
		gStateMachine:change('serve', {
			paddle = Paddle(self.currentPaddle),
			bricks = LevelMaker.createMap(1),
			health = 3,
			score = 0,
			highScores = self.highScores,
			level = 1
		})
	end
	
	-- press escape to quit game
	if love.keyboard.wasPressed('escape') then
		love.event.quit()
	end
end

function PaddleSelectState:render()
	-- draw the text, paddle, and arrows
	-- darken the arrow when ever players scrolled to an end of the paddle list
	...
end
```
___

# breakout-13: music

just in `main.lua`, do something like
* `sounds['music']:setLooping(true)`
* `sounds['music']:play()`
___
