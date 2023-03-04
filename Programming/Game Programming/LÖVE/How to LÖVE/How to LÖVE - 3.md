# Images

let's use this one
![sheep.png (78×100) (sheepolution.com)](https://sheepolution.com/images/book/12/sheep.png)

* needed to be *.png*
* needed to be in the same folder as `main.lua`
	* or sub-directory to where `main.lua` is

* load the image and store it in a variable
```lua
function love.load()
	-- 1.
	myImage = love.graphics.newImage("sheep.png")
	
	-- 2.
	myImage = love.graphics.newImage("path/to/sheep.png")
end
```

* draw the image
```lua
function love.draw()
	love.graphics.draw(myImage, 100, 100)
end
```


### `.draw()` arguments
[love.graphics.draw - LOVE (love2d.org)](https://love2d.org/wiki/love.graphics.draw)

let's take a look at all the arguments of `love.graphics.draw()`
* all besides the image are optional
	* so they're called *optional arguments*
* `love.graphics.print()` for drawing text also have these same arguments

`image`
* The image we want to draw

`x` and `y`
* position of the image

`r`
* rotation, or angle, of the image
* in radians

`sx` and `sy`
* the x-scale and y-scale
* eg. to make the image twice as big
	* `love.graphics.draw(myImage, 100, 100, 0, 2, 2)`
* eg. to mirror an image
	* `love.graphics.draw(myImage, 100, 100, 0, -1, 1)`

`ox` and `oy`
* the x-origin and y-origin of the image
* by default, all the scaling and rotating is based on the top-left of the image
	* the zero position
![origin1.png (327×231) (sheepolution.com)](https://sheepolution.com/images/book/12/origin1.png)
* we need do put the origin some where else to make the scaling and rotating behave differently
	* eg. `love.graphics.draw(myImage, 100, 100, 0, 2, 2, 39, 50)`
![origin2.png (276×187) (sheepolution.com)](https://sheepolution.com/images/book/12/origin2.png)

`kx` and `ky`
* shearing of the image
![shear.png (284×181) (sheepolution.com)](https://sheepolution.com/images/book/12/shear.png)


### Image object
* `love.graphics.newImage()` actually returns an object, an Image object
* [Image - LOVE (love2d.org)](https://love2d.org/wiki/Image)

* It has functions that we can use to edit the image or get data about it
* eg. `:getWidth()` and `:getHeight()`

* to draw an image with the origin being in the center
```lua
function love.load()
	myImage = love.graphics.newImage("sheep.png")
	width = myImage:getWidth()
	height = myImage:getHeight()
end

function love.draw()
	love.graphics.draw(myImage, 100, 100, 0, 2, 2, width/2, height/2)
end
```


### Color
`love.graphics.setColor(r, g, b)` can set the color for everything we draw
* not only images but rectangles, shapes, and lines as well

although RGB-system officially ranges from 0 to 255
LÖVE ranges it from 0 to 1
* instead of `(255, 200, 40)`, we would use `(1, 0.78, 0.15)`
	* `number / 255`
* there is also the forth argument `a` for alpha
	* transparency of everything we draw

remember to set the color back to white if we don't want the color for any other draw calls

```lua
function love.load()
	myImage = love.graphics.newImage("sheep.png")
	-- set background color
	love.graphics.setBackgroundColor(1, 1, 1)
end

function love.draw()
	-- 1.
	love.graphics.setColor(255/255, 200/255, 40/255, 127/255)
	-- 2.
	love.graphics.setColor(1, 0.78, 0.15, 0.5)
	
	-- colored image
	love.graphics.draw(myImage, 100, 100)
	-- reset color for the next image
	love.graphics.setColor(1, 1, 1) -- no alpha argument will automatically set it to 1
	love.graphics.draw(myImage, 200, 100)
end
```
___

# Detect Collision

let's say we're making a game where we can shoot down monsters.
* A monster should die when it is hit by a bullet
* we need to check: is the monster colliding with a bullet?

we're gonna need a collision check function
* which will check collision between rectangles
	* called AABB collision
* we need to know: when do two rectangles collide?

![rectangles1.png (710×252) (sheepolution.com)](https://sheepolution.com/images/book/13/rectangles1.png)

In 1, why the two rectangles are not colliding?
* what is the definition for collision in this example?
* **Red's right side** need to be further to the **right** than **Blue's left side**

But if we apply the definition above in 2, they are also colliding, which is not true apparently
So we need more conditions for the definition of a collision
* **Red's left side** need to be further to the **left** than **Blue's right side**

We have 2 conditions now, is that enough?
Well no.
![rectangles2.png (121×229) (sheepolution.com)](https://sheepolution.com/images/book/13/rectangles2.png)

so 2 more conditions to go
* **Red's bottom side** need to be further to the **bottom** than **Blue's top side**
* **Red's top side** need to be further to the **top** than **Blue's bottom side**

let's experiment the definition a bit
Are all 4 conditions true for these exmaples?
![rectangles3.png (677×230) (sheepolution.com)](https://sheepolution.com/images/book/13/rectangles3.png)

Yes, they are! Now we're done using our brain.
Let's turn it into code.

1. create 2 rectangles
```lua
function love.load()
	r1 = {
		x = 10,
		y = 100,
		width = 100,
		height = 100
	}
	
	r2 = {
		x = 250,
		y = 120,
		width = 150,
		height = 120
	}
end

function love.update(dt)
	-- make r1 moves
	r1.x = r1.x + 100 * dt
end

function love.draw()
	love.graphics.rectangle("line", r1.x, r1.y, r1.width, r1.height)
	love.graphics.rectangle("line", r2.x, r2.y, r2.width, r2.height)
end
```

2. now we build the function for checking collision
```lua
-- it takes 2 rectangles as parameters
function checkCollision(a, b)
	-- with locals it's common to use underscores _ instead of camelCasing
	
	-- get the sides of the rectangles
	local a_left = a.x
	local a_right = a.x + a.width
	local a_top = a.y
	local a_bottom = a.y + a.height
	
	local b_left = b.x
	local b_right = b.x + b.width
	local b_top = b.y
	local b_bottom = b.y + b.height
	
	-- here comes the conditions
	if a_right > b_left
	and a_left < b_right
	and a_bottom > b_top
	and a_top < b_bottom then
		-- there is a collision
		return true
	else
		-- there isn't a collision
		return false
end
```

* since if-condition itself is a boolean value, we can actually skip the if-statement
```lua
function checkCollision(a, b)
	...
	return a_right > b_left
	   and a_left < b_right
	   and a_bottom > b_top
	   and a_top < b_bottom
end
```

3. try the function
```lua
function love.draw()
	local mode
	if checkCollision(r1, r2) then
		-- if there is collision, draw the rectangles filled
		mode = "fill"
	else
		-- else, draw the rectangles as a line
		mode = "line"
	end
	
	love.graphics.rectangle(mode, r1.x, r1.y, r1.width, r1.height)
	love.graphics.rectangle(mode, r2.x, r2.y, r2.width, r2.height)
```
___

# Game: Shoot the enemy
[Sheepolution - How to LÖVE - Chapter 14 - Game: Shoot the enemy](https://sheepolution.com/learn/book/14)

`love.graphics.getWidth()`
`love.graphics.getHeight()`
* gets the width / height of the window

`self.speed = -self.speed`
* makes the instance move to the opposite direction
* `speed` becomes `-speed`
	* `1` becomes `-1`
	* `-1` becomes `1`

`love.load()`
* restart the game

> [!tips]
> A game is essentially a bunch of problems that need to be solved.

___

# Distribute the game
now we share our game with others.
* others don't need to install LÖVE to play our game on their computer
* [Game Distribution - LOVE (love2d.org)](https://www.love2d.org/wiki/Game_Distribution)

1. change the title and icon
	* create a config file called `conf.lua`
```lua
--! file: conf.lua
function love.conf(t)
	t.window.tilte = "Panda Shooter!"
	t.window.icon = "panda.png"
end
```

* LÖVE load `conf.lua` before it starts the game, and applies the configurations
	* see the [full list](https://love2d.org/wiki/Config_Files) of configure options

2. make an executable
* for that we first need to package the project in a zip file
	* but we need to change the extension to `.love` instead of the default `.zip`
![compress.png (665×461) (sheepolution.com)|600](https://sheepolution.com/images/book/15/compress.png)

* this is the Windows way, see [here](https://www.love2d.org/wiki/Game_Distribution) for doing that in other platforms

3. Download [builder.zip](https://drive.google.com/file/d/1xX49nDCI0UxjnwY3-h0f-kpdmHVmNqvz/view) and unzip it
* move the `.love` file on top of `build.bat`
* then it creates a `.zip` file in the same folder

4. Distribute!
* send the `.zip` file to people
	* they have to extract all the files in a folder and open the `.exe` file
* maybe share it on [itch.io](https://itch.io/)
* there was once a [client](https://castle.games/) for sharing LÖVE games
___

# Angle
let's make a circle that moves in the direction of the mouse cursor

to do that, we need to know the angle between
* we can use a function, `math.atan2()`
	* first argument is the target's y position minus the object's y position
	* second argument is the same but for the x-position
* it takes the vertical and horizontal vector, and returns an angle
	* vector = distance + direction

![atan2.png (300×177) (sheepolution.com)](https://sheepolution.com/images/book/16/atan2.png)

```lua
function love.update(dt)
	--love.mouse.getPosition() returns the x and y position of the cursor
	mouse_x, mouse_y = love.mouse.getPosition()
	
	angle = math.atan2(mouse_y - circle.y, mouse_x - circle.x)
end

function love.draw()
	love.graphics.circle("line", circle.x, circle.y, circle.radius)
	
	-- print the angle
	love.graphics.print("angle: " .. angle, 10, 10)
	
	-- some lines to visualize the velocities
	love.graphics.line(circle.x, circle.y, mouse_x, circle.y)
	love.graphics.line(circle.x, circle.y, circle.x, mouse_y)
	
	love.graphics.line(circle.x, circle.y, mouse_x, mouse_y)
end
```
![angle.gif (451×208) (sheepolution.com)](https://sheepolution.com/images/book/16/angle.gif)

### radians and Pi
notice the angle is not going higher than 3.14 (Pi, $\pi$)
* it's because `math.atan2()` returns angle in radians
	* instead of degrees, which we usually see
* angle is between -3.14 and 3.14

* learn what radian is
![radian.gif (450×450) (sheepolution.com)](https://sheepolution.com/images/book/16/radian.gif)
* 360 degrees equals $\pi\times2$
* 90 degrees equals $\pi/2$

* learn what $\pi$, Pi, is
![pi.gif (720×228) (sheepolution.com)](https://sheepolution.com/images/book/16/pi.gif)
* if the diameter of a circle is 1
* it's length will be $\pi$

In Lua we can get $\pi$ by using `math.pi`


### sine and cosine

if we need to make our circle move towards the mouse
we actually do need `math.cos()` and `math.sin()`
* both functions return a number between -1 and 1, based on the angle we pass

here explains how sine and cosine work
<span>
	<img src="https://sheepolution.com/images/book/16/sinecosine2.png">
	<img src="https://sheepolution.com/images/book/16/sinecosine3.png">
	<img src="https://sheepolution.com/images/book/16/sinecosine4.png">
</span>
* if the angle would point to the left
	* then cosine would be -1 and sine would be 0
* if the angle would point down
	* then cosine would be 0 and sine would be 1

![sinecosine.gif (250×253) (sheepolution.com)](https://sheepolution.com/images/book/16/sinecosine.gif)

so how they can move the circle towards the mouse?
* if the mouse is at a diagonal upper right angle
	* sine would be -0.7 and cosine would be 0.7
* multiply x by cosine to make the circle moves right
* multiply y by sine to make the circle moves up

* then it can move the circle straight towards our mouse
```lua
function love.update(dt)
	mouse_x, mouse_y = love.mouse.getPosition()
	
	angle = math.atan2(mouse_y - circle.y, mouse_x - circle.x)
	
	cos = math.cos(angle)
	sin = math.sin(angle)
	
	circle.x = circle.x + circle.speed * cos * dt
	circle.y = circle.y + circle.speed * sin * dt
end

function love.draw()
	love.graphics.circle("line", circle.x, circle.y, circle.radius)
	
	love.graphics.line(circle.x, circle.y, mouse_x, mouse_y)
	love.graphics.line(circle.x, circle.y, mouse_x, circle.y)
	love.graphics.line(circle.x, circle.y, circle.x, mouse_y)
end
```
![following_circle.gif (781×259) (sheepolution.com)](https://sheepolution.com/images/book/16/following_circle.gif)
___

# Distance

now let's say we only want to move the circle when it's near our cursor
* we need to calculate the distance between them

now this is Pythagorean theorem (畢氏定理)
* we can calculate the longest line in a triangle with a right angle
<img src="https://sheepolution.com/images/book/16/pythagorean.png" style="background-color: white;">

1. use the length of the shorter sides to make 2 squares
2. sum them up to make one big square
3. find the sqaure root of that big square, then it's the length of the longest line
	* the longest line is also known as the *hypotenuse*

so how does this help finding the distance?
well, if you have 2 points, it can form an invisible triangle
![triangle.png (439×321) (sheepolution.com)](https://sheepolution.com/images/book/16/triangle.png)
* so the hypotenuse is actually the distance between 2 points

let's turn Pythagorean theorem into code
```lua
function getDistance(x1, y1, x2, y2)
	local horizontal_distance = x1 - x2
	local vertical_distance = y1 - y2
	
	-- both work the same
	local a = horizontal_distance * horizontal_distance
	local b = vertical_distance ^ 2
	
	local c = a + b
	-- math.sqrt() finds the square root
	local distance = math.sqrt(c)
	return distance
end

function love.draw()
	love.graphics.circle("line", circle.x, circle.y, circle.radius)
	love.graphics.line(circle.x, circle.y, mouse_x, mouse_y)
	love.graphics.line(circle.x, circle.y, mouse_x, circle.y)
	love.graphics.line(mouse_x, mouse_y, mouse_x, circle.y)
	
	-- visualize the distance with a circle
	local distance = getDistance(circle.x, circle.y, mouse_x, mouse_y)
	love.graphics.circle("line", circle.x, circle.y, distance)	
end
```
![following_circle_distance.gif (781×259) (sheepolution.com)](https://sheepolution.com/images/book/16/following_circle_distance.gif)

let's make the circle only move when it's close enough, and the closer it gets the slower it moves
* just for fun
```lua
function love.update(dt)
	mouse+x, mouse_y = love.mouse.getPosition()
	angle = math.atan2(mouse_y - circle.y, mouse_x, circle.x)
	cos = math.cos(angle)
	sin = math.sin(angle)
	
	local distance = getDistance(circle.x, circle.y, mouse_x, mouse_y)
	
	if distance < 400 then
		circle.x = circle.x + circle.speed * cos * (distance/100) * dt
		circle.y = circle.y + circle.speed * sin * (distance/100) * dt
	end
end
```
![following_circle_distance_speed.gif (781×259) (sheepolution.com)](https://sheepolution.com/images/book/16/following_circle_distance_speed.gif)
___

# Move an arrow image

last thing, let's use an image instead of the boring circle
* firstly, what direction the arrow in the image should point to?

with an angle of the default 0, cosine is 1, and sine is 0
* so x is 1, y is 0
* that mean objects, or the image, would move to right if angle is 0
	* so the arrow in the image should point to the right
![arrow_right.png (100×100) (sheepolution.com)](https://sheepolution.com/images/book/16/arrow_right.png)
```lua
function love.load()
	arrow = {}
	arrow.x = 200
	arrow.y = 200
	arrow.speed = 300
	arrow.angle = 0
	arrow.image = love.graphics.newImage("arrow_right.png")
end

function love.update(dt)
	mouse_x, mouse_y = love.mouse.getPosition()
	arrow.angle = math.atan2(mouse_y - arrow.y, mouse_x - arrow.x)
	cos = math.cos(arrow.angle)
	sin = math.sin(arrow.angle)
	
	arrow.x = arrow.x + arrow.speed * cos * dt
	arrow.y = arrow.y + arrow.speed * sin * dt
end

function love.draw()
	love.graphics.draw(arrow.image, arrow.x, arrow.y, arrow.angle)
	love.graphics.circle("fill", mouse_x, mouse_y, 5)
end
```

now if you run this, the arrow would be a bit off
* because the image is rotating around its top-left, instead of its center
![arrow_off.png (306×276) (sheepolution.com)](https://sheepolution.com/images/book/16/arrow_off.png)

after fixing it'll work fine
```lua
function love.load()
	arrow = {}
	...
	arrow.origin_x = arrow.image:getWidth() / 2
	arrow.origin_y = arrow.image:getHeight() / 2
end

function love.update(dt)
	...
end

function love.draw()
	love.graphics.draw(arrow.image,
		arrow.x, arrow.y, arrow.angle, 1, 1,
		arrow.origin_x, arrow.origin_y)
	love.graphics.circle("fill", mouse_x, mouse_y, 5)
end
```
![following_arrow.gif (781×259) (sheepolution.com)](https://sheepolution.com/images/book/16/following_arrow.gif)
___
