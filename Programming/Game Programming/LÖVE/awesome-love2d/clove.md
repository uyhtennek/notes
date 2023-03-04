[besnoi/clove: A helper library for LÖVE which allows to loads huge amount of assets super-easily (github.com)](https://github.com/besnoi/clove)
* easily loads huge amount of assets
	* images, fonts, sounds, and videos
* easily requireing libraries
___

# How to use Clove?

1. `clove = require "clove"`

2. prepare images
* file names are `"A.png"`, `"B.png"`, and so on
* folder is named `"asset/"`
![img_keyboard.png (891×492) (raw.githubusercontent.com)|680](https://raw.githubusercontent.com/besnoi/clove/master/Images/img_keyboard.png)

3. `images = clove.importImages("asset")`
___

# Load an arbitrary asset
CLove doesn't care what the type of asset it is

eg. loading sound
* love - `asset = love.audio.newSource("audio.ogg", "static")`
* clove - `asset = clove.loadAsset("audio.ogg")`
	* same as `asset = clove.importAsset("audio.ogg")`
	* default uses `"static"` in this case
		* use `asset = clove.loadAsset("audio.ogg", "stream")` instead if `"stream"` is preferred

eg. loading font
* love - `font = love.graphics.newFont("Kenny Mini.ttf", 45`
* clove - `font = clove.loadAsset("Kenney Mini", 45)`
	* yeah no need `.ttf` or `.otf`
___

# Load up all assets at once

we have an `"assets/"` folder structured as such
![img_asset.png (324×451) (raw.githubusercontent.com)](https://raw.githubusercontent.com/besnoi/clove/master/Images/img_asset.png)

use `clove.importAll("assets", true, _G)`
```lua
-- example
clove = require 'clove'

clove.importAll("assets", true, _G)
ready:play()

function love.draw()
	love.graphics.draw(mountain_range)

	love.graphics.setFont(Kenney_Mini)
	love.graphics.print("Hello World")
end
```

* `true` as the second argument means we also import sub-directories
	* be careful not to name files and folders with the same name
		* ng 1. a folder with another file with the same name
		* ng 2. a file in a folder and another file in another folder, named the same
	* the latter will vanish into space

* if passed `_G` as the third argument, make sure the asset name is not something like `'jit.mp3'` or `'rawset.png'` or the like
	* that parameter is the table we want to add the assets to
	* `_G` means *global assets*
	* by default it's just a `{}`
___

# Load specific type of assets

```lua
clove.importFonts()    -- fonts
clove.importGraphics() -- images
clove.importAudio()    -- sounds
clove.importVideo()    -- videos
```
___
