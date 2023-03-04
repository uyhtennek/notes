# GUIs
*graphical user interface*
eg. common widgets and elements include Panels, Labels, Textboxes, Progress bars, and others
___

## Nine Patch

We use a construct called a *Nine Patch*
* the idea is that images can scale to a large size if we have a image with 9 pieces
	* this is where the terminology comes from
* also, usually the image is very small
* back in the 80s and 90s, hardware was fundamentally tile based
	* games 'til these days still use this method

![fetch.php (64×64) (rpginabox.com)](https://www.rpginabox.com/docs/lib/exe/fetch.php?media=wiki:dialogue_box_9patch_01.png)

![fetch.php (580×198) (rpginabox.com)](https://www.rpginabox.com/docs/lib/exe/fetch.php?media=wiki:dialogue_box_9patch_02.png)

for the parts (most likely the edge and the center) that we want to extend, we can either
* tile it a bunch of times
* or just stretch it
	* some bonuses for stretching is that depending how we set the filter mode, we can achieve different visual effects
		* `love.graphics.setDefaultFilter()`
		* `'bilinear'`, we can get a nice gradient
		* `'nearest'`, we can get a nice pixelated look

We will see this often
* Unity has nice support for this
* but we won't see it in the lecture though
___

# Selection

So a selection box is just a rectangle with some texts or labels on it, maybe a cursor pointing to the current highlighted option as well
* but they are just a list of things that get rendered, how do we make them do something when the user pressed Enter?

in terms of funtionality, we just care about the selections itself
* the selections point to different callback functions
* when the user pressed Enter, it executes the corresponding callback

eg. Fight
1. chose Fight, then the callback would be something like push a battle state onto the state stack
2. then the 2 entities, would do a series of actions or interactions automatically
	* like the first one would attack the second one, then the second one would attack the first one, then ends the turn
* those are some other asynchronous set of states as well, that do its own thing
	* kicked off by our initial callback for the selection

eg. Item
1. pushes an Item Menu State, which opens another brand new set of menus
2. each of the items in the menu, has its own callback associated
3. for instance we chose a potion
4. it opens another state, which allows us to choose which Pokemon to restore
5. again, each Pokemon has a callback associated

> [!info]
> To get all of these rather complex behaviors, ultimately we're keep pushing more states, and making more callback functions.

eg. Run
1. push a Fade In State
2. pop the Fade In State, pop the Battle State
3. push a Fade Out State
___

# Progress Bar

It's just 2 rectangles
* in addition, Love2D allows us to set the edges on rectangles, to be rounded or not
	* via an optional parameter
* one set to `'fill'`, one set to `'line'`

Then, how to map the Pokemon HP into the progress bar?
* calculating ratio, given the width of the porgress bar and the current and max HP
___

# Panel

2 rectangles.
* the background
* a slightly smaller but darker rectangle
	* it blocks the large part of the background so the background becomes the border

```lua
--! file: Panel.lua
Panel = Class{}

function Panel:init(x, y, width, height)
	self.x = x
	self.y = y
	self.width = width
	self.height = height
	
	self.visible = true
end

function Panel:update(dt)

end

function Panel:render()
	if self.visible then
		love.graphics.setColor(255, 255, 255, 255)
		ove.graphics.rectangle('fill', self.x, self.y, self.width, self.heihgt, 3)
		love.graphics.setColor(56, 56, 56, 255)
		ove.graphics.rectangle('fill', self.x + 2, self.y + 2, self.width - 4, self.heihgt - 4, 3)
		love.graphics.setColor(255, 255, 255, 255)
	end
end

function Panel:toggle()
	self.visible = not self.visible
end
```
___

## Textbox

a little more complicated than a panel
* it needs to take in some arbitrary body of text, then chop it up, or wrap it, based on how wide the text box is
* and if its height surpasses the height of the text box, ideally, we should page the text
	* player would need to press Space bar or Enter to go through pages of text until it exhausted all of the text
* then when there is no text left to read, press the Enter one last time, to close the text box

```lua
--! file: Textbox.lua
function Textbos:init(x, y, width, height, text, font)
	self.panel = Panel(x, y, width, height)
	self.x = x
	self.y = y
	self. width = width
	self.height = height
	
	self.text = text
	self.font = font or gFonts['small']
	_, self.textChunks = self.font:getWrap(self.text, self.width - 12)
	
	self.chunkCounter = 1
	self.endOfText = false
	self.closed = false
	
	self:next()
end

function Textbox:nextChunks()
	local chunks = {}
	
	-- we display only 3 lines for every page
	for i = self.chunkCounter, self.chunkCounter + 2 do
		table.insert(chunks, self.textChunks[i])
		
		if i == #self.textChunks then
			-- if there is no much available chunks
			self.endOfText = true
			return chunks
		end
	end
	
	-- go to the next page
	self.chunkCounter = self.chunkCounter + 3
	return chunks
end

function Textbox:next()
	if self.endOfText then
		-- are we at the end of the text?
		self.displayingChunks = {}
		self.panel:toggle()
		self.closed = true
	else
		self.displayingChunks = self:nextChunks()
	end
end

function Textbox:update(dt)
	if ... then -- if 'space' or 'enter'
		self:next()
	end
end

function Textbox:isClosed()
	return self.closed
end

function Textbox:render()
	-- render the panel
	self.panel:render()
	
	love.graphics.setFont(self.font)
	-- render the 3 lines
	for i = 1, #self.displayingChunks do
		love.graphics.print(self.displayingChunks[i], self.x + 3, self.y + 3 + (i - 1) * 16)
	end
end
```

`_, self.textChunks = self.font:getWrap(self.text, self.width - 12)`
* it chops the text to chunks and put them into a table
* based on how wide its given width is
	* no piece of text will ever excedd `self.width - 12` width
___

# Selection - code

it's a list of text items, with a cursor
* each of the items has a callback function

```lua
--! file: Selection.lua
Selection = Class{}

function Selection:init(def)
	self.items = def.items -- we expect each def.items to have a callback function
	...
end

function Selection:update(dt)
	if love.keyboard.wasPressed('up') then
		if self.currentSelection == 1 then
			-- we're at the top one so for the next one, go to the bottom one
			self.currentSelection = #self.items
		else
			self.currentSelection = self.currentSelection - 1
		end
	elseif love.keyboard.wasPressed('down') then
		if self.currentSelection == #self.items then
			-- we're at the bottom one so for the next one, go to the top one
			self.currentSelection = 1
		else
			self.currentSelection = self.currentSelection + 1
		end
	...
	elseif ... then -- pressed 'enter' or 'return'
		-- expect items to have a onSelect callback
		self.items[self.currentSelection].onSelect()
		
		gSounds['blip']:stop()
		gSounds['blip']:play()
	end
end

function Selection:render()
	local currentY = self.y
	
	for i = 1, #self.items do
		local paddedY = currentY + ... -- calculate how much padding that we need
		
		-- draw cursor
		if i == self.currentSelection then
			love.graphics.draw(gTextures['cursor'], self.x - 8, paddedY)
		end
		
		-- draw the item
		love.graphics.printf(self.items[i].text, self.x, paddedY, self.width, ...)
		
		currentY = currentY + gapHeight
	end
end
```
___

# Menu

it's a Panel and a Selection combined
* we can make complicated menu, for this example we'd keep it simple

```lua
--! file: Menu.lua
Menu = Class{}

function Menu:init(def)
	self.panel = Panel(def.x, def.y, def.width, def.height)
	self.selection = Selection {
		items = def.items,
		x = def.x, y = def.y, width = def.width, height = def.height
	}
end

function Menu:update(dt)
	self.selection:update(dt)
end

function Menu:render()
	self.panel:render()
	self.selection:render()
end
```
___
