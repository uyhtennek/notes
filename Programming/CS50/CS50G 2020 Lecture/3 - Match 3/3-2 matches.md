# swap-0: the board

```lua
-- for the GenerateQuads() function
require 'Util'

function love.load()
	tileSprite = love.graphics.newImage('match3.png')
	tileQuads = GenerateQuads(tileSprite, 32, 32)
	
	board = generateBoard()
	...
end

-- a lot like the LevelMaker in Breakout
function generateBoard()
	local tiles = {}
	--[[tiles is a table like
	{
		{0, 1, 1, 1},
		{1, 1, 1, 1},
		{1, 1, 1, 1}
	}]]
	
	-- since tiles[1] gives the rows of the table or a sub-table
	-- it's effectively tiles[y][x] to index to an item, so y is the outer-loop
	for y = 1, 8 do
		table.insert(tiles, {})
		for x = 1, 8 do
			table.insert(tiles[y], {
				x = (x - 1) * 32,
				y = (y - 1) * 32
				tile = math.random(#tileQuads)
			})
		end
	end
	
	return tiles
end

function love.draw()
	drawBoard(128, 16) -- 128 and 16 are x and y, so draw the board at (128, 16)
end

function drawBoard(offsetX, offsetY)
	for y = 1, 8 do
		for x = 1, 8 do
			local tile = board[y][x]
			love.graphics.draw(tileSprite, tileQuads[tile.tile], tile.x + offsetX, tile.y + offsetY)
		end
	end
end
```
___

# swap-1: the swap
recall [[CS50x 4-3 Playing with Memory#swap()|how to do swap]] from CS50x

```lua
...
function love.load()
	...
	highlightedTile = false -- tells do we have a highlighted tile?
	highlightedX, highlightedY = 1, 1
	
	selectedTile = board[1][1]
	...
end

function drawBoard(offsetX, offsetY)
	for y = 1, 8 do
		for x = 1, 8 do
			...
			if highlightedTile then
				if tile.gridX == highlightedX and tile.gridY == highlightedY then
					-- highlight the tile
					love.graphics.setColor(255, 255, 255, 128)
					love.graphics.rectangle('fill', tile.x + offsetX, tile.y + offsetY,
						32, 32, 4) -- 4 rounded segments for the rectangle's corners
					love.graphics. setColor(255, 255, 255, 255)
				end
			end
		end
	end
	
	-- draw selected tile
	love.graphics.setColor(255, 0, 0, 234)
	love.graphics.setLineWidth(4)
	love.graphics.rectangle('line', selectedTile.x + offsetX, selectedTile.y + offsetY,
		32, 32, 4)
	love.graphics.setColor(255, 255, 255, 255)
end

function love.keypressed(key)
	...
	local x, y = selectedTile.gridX, selectedTile.gridY
	
	if key == 'up' then
		if y > 1 then
			selectedTile = board[y - 1][x]
		end
	elseif key == 'down' then
		if y < 8 then
			selectedTile = board[y + 1][x]
		end
	elseif key == 'left' then
		if x > 1 then
			selectedTile = board[y][x - 1]
		end
	elseif key == 'right' then
		if x < 8 then
			selectedTile = board[y][x + 1]
		end
	end
	
	if key == 'enter' or key == 'return' then
		if not highlightedTile then
			highlightedTile = true
			highlightedX, highlightedY = selectedTile.gridX, selectedTile.gridY
		else
			-- crazy code, but just basic swapping
			-- due to many sub fields
			local tile1 = selectedTile
			local tile2 = board[highlightedY][highlightedX]
			
			local tempX, tempY = tile2.x, tile2.y
			local tempgridX, tempgridY = tile2.gridX, tile2.gridY
			
			local tempTile = tile1
			board[tile1.gridY][tile1.gridX] = tile2
			board[tile2.gridY][tile2.gridX] = tempTile
			
			tile2.x, tile2.y = tile1.x, tile1.y
			tile2.gridX, tile2.gridY = tile1.gridX, tile1.gridY
			tile1.x, tile1.y = tempX, tempY
			tile1.gridX, tile1,gridY = tempgridX, tempgridY
			
			highlightedTile = false
			selectedTile = tile2
		end
	end
end
```

## swap constraint
the code above actually allows swapping between any 2 tiles
but in the game we can only swap any 2 **adjacent** tiles

if you're wondering how we can check for adjacent tiles
* just check for `math.abs(tile1.x - tile2.x + tile1.y - tile2.y) == 1`
	* if it's 0, `tile1` and `tile2` are the same tile, or in the same position
	* if it's 2, `tile1` and `tile2` are diagonal
* the only way they are adjacent, it has to be 1
___

# swap-2: the tween swap

```lua
function love.keypressed(key)
	...
	if key == 'enter' or key == 'return' then
		...
	else
		...
		Timer.tween(0.2, {
			[tile2] = {x = tile1.x, y = tile1.y},
			[tile1] = {x = tempX, y = tempY}
		})
		...
	end
end
```
___

# match-0: find matches

how do we find matches?
* just iterate through the board, every row and every column
* so 2 big loops: left-to-right, and top-to-bottom

for each iteration, we have a int variable `numberOfMatches`, which has a value of 1
* start off the first tile, and go to the next
* does it has the same color?
	* yes. then `numberOfMatches` increases by 1
		* if it gets to 3, add the tiles to a group, as a list of matches
	* no. then `numberOfMatches` get back to 1
* if we're at the second tile before the end of a row or column, we can effectively skip the remaining check
	* the remaining 2 tiles can't form a match any way, so no point in checking
	* just a shortcut

how about matches like a cross or a T-shape?
* 3 in a row and 3 in a column, at least 1 of the tiles is the intersection of the 2 matches.
* since we only delete tiles after all the loops are done, it still count as 2 matches

```lua
--! file: Board.lua
...
function Board:calculateMatches()
	local matches = {}
	local matchNum = 1
	
	-- horizontal matches
	for y = 1, 8 do
		local colorToMatch = self.tiles[y][1].color
		matchNum = 1
		
		for x = 2, 8 do
			if self.tiles[y][x].color == colorToMatch then
				-- same color
				matchNum = matchNum + 1
			else
				-- different color
				colorToMatch = self.tiles[y][x].color
				
				if matchNum >= 3 then
					local match = {}
					
					for x2 = x - 1, x - matchNum, -1 do
						-- go backwards to store the matching tiles
						table.insert(match, self.tiles[y][x2])
					end
					
					table.insert(matches, match)
				end
				
				if x >= 7 then
					-- we're already at the second last tile in a row
					break
				end
				
				matchNum = 1
			end
		end
		
		if matchNum >= 3 then
			-- this match is at the end of the row
			local match = {}
			for x = 8, 8 - matchNum + 1, -1 do
				table.insert(match, self.tiles[y][x])
			end
			
			table.insert(matches, match)
		end
	end
	
	-- vertical matches, just same logic
	for x = 1, 8 do
		local colorToMatch = self.tiles[1][x].color
		matchNum = 1
		
		for y = 2, 8 do
			...
		end
		...
	end
	
	-- we keep the references so later we can remove them, or check the pattern
	self.matches = matches
	-- if there are matches, we later call some functions to bring in new tiles
	return #self.matches > 0 and self.matches or false
end
```
___

# match-1: remove matches

how to remove tiles?
* just set them to `nil`

then, how do we shift the upper tiles down, to the places where the removed tiles are?
* again, we iterate through each column, but this time we'd start from the bottom
* any time we see a `nil`, we look for any upper tile, and shift it down
* after shifting, we need to start from the 1 upper tile of the shifted tile
	* instead of where the shifted tile used to be
	* because there could be several `nil` tiles between the first `nil` tile and its upper actual tile
* almost like a [[CS50x 3-4 Sorting#Bubble Sort|bubble sort]]

```lua
--! file: Board.lua
function Board:getFallingTiles()
	for x = 1, 8 do
		local space = false -- flag that tells if we have a space tile already
		local spaceY = 0 -- 0 to sort of indicate that we don't have a space tile yet
		
		local y = 8
		while y >= 1 do
			local tile = self.tiles[y][x] -- it'd be nil if the tile is removed
			
			if space then
				-- we already found a space / empty tile
				if tile then
					-- we have an actual in the current position
					-- move it to the space we found
					self.tiles[space][x] = tile
					tile.gridY = spaceY
					
					self.tiles[y][x] = nil
					
					-- set up for the falling tweening behavior
					tweens[tile] = {
						y = (tile.gridY - 1) * 32
					}
					
					space = false
					y = spaceY
					
					spaceY = 0
				end
			else if tile == nil then
				-- we found a space (a tile that's nil)
				space = true
				
				if spaceY == 0 then
					spaceY = y
				end
			end
			
			y = y - 1
		end
	end
	
	-- create replacement tiles at the top of the screen
	for x = 1, 8 do
		for y = 8, 1, -1 do
			...
		end
	end
	
	return tweens
end
```
___

# match-2: replace tiles

we just need to spawn the correct number of new tiles in each column
* need to make them fall from above as well though

remember the data structure and the coordinate are two seperate things
* we can still put the new tiles at the right spot in the `board` table
* but tween their position to make it falls from above
* it's better to disable input though, while it's replacing
	* so less likely to cause visual bugs

also do remember to find matches again every time the replacements are finished
```lua
--! file: PlayState.lua
...
function PlayState:calculateMatches()
	self.highlightedTile = nil
	
	local matches = self.board:calculateMatches()
	if matches then
		gSounds['match']:stop()
		gSounds['match']:play()
		
		for k, match in pairs(matches) do
			self.score = self.score + #match * 50
		end
		
		self.board:removeMatches()
		
		local tilesToFall = self.board:getFallingTiles()
		Timer.tween(0.25, tilesToFall):finish(function()
			-- the falling tweens are finished
			self:calculateMatches()
		end)
	else
		-- no more new matches any more, so give the player control
		self.canInput = true
	end
end
...
```
___
