[Nikaoto/deep: Queue and execute actions in sequence (add Z axis to 2D graphics frameworks) (github.com)](https://github.com/Nikaoto/deep)
* queuing and executing actions in sequence
* can be used in multiple ways
	* one of which is as a Z-axis, to perform drawing in order
___

```lua
deep = require "deep"

deep.queue(3, print, "wound!")
deep.queue(1, print, "It is just")
deep.queue(2, print, "a flesh")

deep.execute()

-- Output
It is just
a flash
wound!
```

```lua
local deep = require "deep"

-- The z index of the red cube
local current_z = 1

-- Draws a horizontal line at passed y coordinate
local function draw_rectangle(y)
  love.graphics.setColor(1, 1, 1) -- Set color to white
  love.graphics.rectangle("fill", 200, y, 300, 10) -- Draw thin rectangle
end

function love.draw()
  -- Queue the three horizontal lines
  deep.queue(2, draw_rectangle, 60)
  deep.queue(3, draw_rectangle, 80)
  deep.queue(4, draw_rectangle, 100)

  -- Red square, which can move through z axis
  deep.queue(current_z, function()
    love.graphics.setColor(1, 0, 0) -- Set color to red
    love.graphics.rectangle("fill", 300, 40, 80, 80)
  end)

  -- Draw everything in the queue
  deep.execute()
end

-- Increases/decreases red square z on key press
function love.keypressed(key)
  if key == "up" then
    current_z = current_z + 1
  elseif key == "down" then
    current_z = current_z - 1
  end
end
```

![68747470733a2f2f692e696d6775722e636f6d2f796b324f31616f2e676966 (400×165) (camo.githubusercontent.com)](https://camo.githubusercontent.com/ca9fc4315a61c17885e6bd5aac4f85bc8c20eba0f3c92eab22b76b924701dbae/68747470733a2f2f692e696d6775722e636f6d2f796b324f31616f2e676966)
