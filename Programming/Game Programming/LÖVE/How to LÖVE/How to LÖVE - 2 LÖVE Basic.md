# Moving a rectangle

1. prepare the 3 main callbacks
```lua
function love.load()
	
end

function love.update()
	
end

function love.draw()
	
end
```

2. draw a rectangle
```lua
function love.draw()
	love.graphics.rectangle("line", 100, 50, 200, 150)
end
```

* second and third arguments are the x and y position
	* 0 for x is the left side of the screen
	* 0 for y is the top side of the screen

<center>
	<span>
		<img src="https://sheepolution.com/images/book/5/coordinates.png" height="250">
	</span>
</center>

3. move the rectangle
```lua
function love.load()
	x = 100
end

function love.update()
	x = x + 5
end

function love.draw()
	love.graphics.rectangle("line", x, 50, 200, 150)
end
```


### Delta time
the function `love.update()` has a hidden parameter, `dt` (short for *delta time*)

the thing is, if we were to run the game on a different computer, the rectangle might move with a different speed
* because not all computers update the program at the same rate
* the same frame rate, or fps (frames per second)

Delta time is the time that has passed between the previous and the current update
* for a computer running with 100 fps, delta time on average would be 1 / 100, which is 0.01
* for a computer running with 200 fps, delta time on average would be 1 / 200, which is 0.005

so in 1 second
* computer A updates 100 times and increases `x` by 5 x 0.01 each frame
* computer B updates 200 times and increases `x` by 5 x 0.005 each frame
* in the end, both computers increase `x` by 5 in 1 second
	* $100\times5\times0.01=5$
	* $200\times5\times0.005=5$

```lua
function love.update(dt)
	x = x + 5 * dt
end
```
___

# If-statements

```lua
if condition then
	-- code
end
```
* if-statements check if the statement is NOT `false` or `nil`
	* if not, then execute
	* if `false` or `nil`, then won't execute

* to stop to rectangle from moving off screen, we may use
```lua
function love.update(dt)
	if x < 600 then
		x = x + 100 * dt
	end
end
```

**operators for compairing**
```lua
==
~=
<=
>=
<
>
```

### Boolean
* `true` or `false`

* let say we assigned a variable `move` with a value `true`
```lua
-- 1.
if move == true then
	x = x + 100 * dt
end

-- 2. same as 1.
if move then
	x = x + 100 * dt
end

-- 3. check if move is false instead
if not move then
	x = x + 100 * dt
end

-- 4. check if a number is not equal to another number
if 4 ~= 5 then
	x = x + 100 * dt
end
```

* we can assign `true` or `false` to a variable with a statement
```lua
move = 6 > 3
```

### Arrow keys
* let's move the rectangle by holding down arrow keys

```lua
function update()
	if love.keyboard.isDown('up') then
		y = y - 100 * dt
	elseif love.keyboard.isDown('down') then
		y = y + 100 * dt
	elseif love.keyboard.isDown('left') then
		x = x - 100 * dt
	elseif love.keyboard.isDown('right') then
		x = x + 100 * dt
	end
end
```

### `and` & `or`

```lua
-- and
if 5 < 9 and 14 > 7 then
	print("Both statements are true")
end

-- or
if 20 < 9 or 14 > 7 or 5 == 10 then
	print("One of these statements is true")
end
```
___

# Tables
* tables are lists of values

* declare a table
```lua
function love.load()
	-- 1.
	fruits = {}
	-- 2.
	fruits = {"apple", "banana"}
end
```

* insert values to a table
```lua
function love.load()
	fruits = {"apple", "banana"}
	table.insert(fruits, "pear")
end
```

* access the values in a table
```lua
function love.draw()
	love.graphics.print(fruits[1], 100, 100)
	love.graphics.print(fruits[2], 100, 200)
	love.graphics.print(fruits[3], 100, 300)
end
--Output:
--apple
--banana
--pear
```

* remove values from a table
```lua
function love.load()
	fruits = {"apple", "banana", "pear", "pineapple"}
	table.remove(fruits, 2) -- remove the second value, which is "banana"
end
```

when we remove a value from a table with `table.remove`
all the following items in the table will move up

<center><img src="https://sheepolution.com/images/book/7/shift.png" height="250"></center>
___

# For-loops

```lua
function love.load()
	for i=1,10 do
		print("hello", i)
	end
end
--Output:
--hello 1
--hello 2
--...
--hello 10
```

* there is a third, optional number, which specifies by how much the variable `i` increases
```lua
function love.load()
	for i=6,18,4 do
		print("hello", i)
	end
end
--Output:
--hello 6
--hello 10
--hello 14
--hello 18
```

* to loop through a table
```lua
function love.load()
	fruits = {"apple", "banana", "pear"}
	
	-- 1. since we know the length of the table
	for i=1,3 do
		print(fruits[i])
	end
	
	-- 2. use #fruits to get the length of the table
	for i=1,#fruits do
		print(fruits[i])
	end
	
	-- 3. printing in game
	for i=1,#fruits do
		love.graphics.print(fruits[i], 100, 100 + 50 * i)
	end
end
```

### `ipairs`
there is another type of for-loop, helps looping through, or *iterate* through, a table

```lua
function love.load()
	fruits = {"apple", "banana", "pear", "pineapple"}
	
	for i,v in ipairs(fruits) do
		print(i, v)
	end
end
```

* `i` tells the position of the table that we're at
	* at which iterations we're at when iterating through the table
* `v` tells the value of that position in the table
	* it's basically a shorthand for `fruits[i]`

it's the shorthand for the following
```lua
for i=1, #fruits do
	v = fruits[i]
end
```
___

# Objects

Tables don't necessarily use numbers as the keys
It can be strings as well
```lua
function love.load()
	rect = {}
	rect["width"] = 100
end
```

`"width"` in this case is what we call a *key* or a *property*
also we don't need to use `rect["width"]` every time, we can also use `rect.width`

let's create the moving rectangle again, but this time we're gonna utilize object
```lua
function love.load()
	rect = {}
	rect.x = 100
	rect.y = 100
	rect.width = 70
	rect.height = 90
	
	rect.speed = 100
end

function love.update(dt)
	rect.x = rect.x + rect.speed * dt;
end
```

and we can make a table filled with tables
let's make a list of rectangles
```lua
function love.load()
	listOfRectangles = {}
end

function createRect()
	rect = {}
	rect.x = 100
	rect.y = 100
	rect.width = 70
	rect.height = 90
	rect.speed = 100
	
	table.insert(listOfRectangles, rect)
end

-- whenever we press space, call createRect() to make a new rectangle
function love.keypressed(key)
	if key == "space" then
		createRect()
	end
end

-- also need to change our update and draw function
function love.update(dt)
	for i,v in ipairs(listOfRectangles) do
		v.x = v.x + v.speed * dt
	end
end

function love.draw(dt)
	for i,v in ipairs(listOfRectangles) do
		love.graphics.rectangle("line", v.x, v.y, v.width, v.height)
	end
end
```

![moving_rectangles.gif (637×245) (sheepolution.com)|600](https://sheepolution.com/images/book/8/moving_rectangles.gif)

### function in objects
an object can also have functions

```lua
-- 1.
tableName.functionName = function()
	
end

-- 2. the more common way
function tableName.functionName()
	
end
```
___

# Multiple files

if we have a second file called `example.lua`
inside of which we declared a variable
```lua
--! file: example.lua
test = 20
```

then we print the variable in our primary file `main.lua`
```lua
--! file: main.lua
print(test)
```

the output is obviously `nil`
because we have to load the file first

we do this by using a `require()` function
```lua
--! file: main.lua
require("example") -- leave out the ".lua", because Lua does this for us
print(test)
--Output: 20
```

this assume `main.lua` and `example.lua` are both in the same directory
if `example.lua` is in a subdirectory, then we use
```lua
-- use . instead of /
require("path.to.example")
```

in this context, it makes `test` a *global variable*
* we can use it everywhere in the project

if we instead declare `test` as
```lua
--! file: example.lua
local test = 20
```

then it's a *local varaible*
this way if we try `print(test)` in `main.lua`, it would be `nil` again
___

# Scope

in the case of `local test`, it's scope is the file `example.lua`
* it can be used everywhere inside that file, but not in other files

similarly if we create a local variable inside a *block*
* like a function, if-statement, or for-loop
* then that would be the variable's scope

```lua
if true then
	local test = 20
end

print(test)
--Output: nil
```

* also parameters of functions are local to the function
* because they are created inside the function
	* recall [[CS50x 4-3 Playing with Memory#swap()|memory allocation and stack]]

### scope cheat sheet

```lua
--! file: main.lua
test = 10
require("example")
print(test)
--Output 10
```

```lua
--! file:example.lua
local test = 20

function some_function(test)
	if true then
		local test = 40
		print(test)
		--Output: 40
	end
	print(test)
	--Output: 30
end

some_function(30)

print(test)
--Output: 20
```

```lua
--Result
40
30
20
10
```

<center><img src="https://sheepolution.com/images/book/9/scope.png"></center>

when creating a local variable, we don't have to assign a value right away
```lua
local test
test = 20
```

### returning a value of files
* if we have a return statement at the top scope of a file, so not in any function
* it'll be returned when we use `require()` to load the file

```lua
--! file: exmaple.lua
local test = 99
return test
```

```lua
--! file: main.lua
local hello = require "example"
print(hello)
--Output: 99
```

### when and why locals?
best practice is to always use local variables
for many reasons

1. Lua is faster with accessing locals than globals
* just one global variable it may not be making any difference
* if there are a lot of globals it quickly adds up

2. with globals it's easier to make mistakes
* like accidentally use the same variable twice at different locations
	* changed the variable in one place but the value doesn't make sense in another place

if we're going to only use a variable in a certain scope,
make it local

```lua
-- 1.
function love.load()
	listOfRectangles = {}
end

-- 2.
local listOfRectangles = {}

function love.load()
	
end

function createRect()
	local rect = {}
	rect.x = 100
	rect.y = 100
	rect.width = 70
	rect.height = 90
	rect.speed = 100
	
	table.insert(listOfRectangles, rect)
end
```

Some people may argue never to use globals.
It's okay to use, but just keep in mind that locals are faster.
___

# Libraries
a library is code that everyone can use to add certain functionality to their project
* eg. [rxi/tick: Lua module for delaying function calls (github.com)](https://github.com/rxi/tick)

1. we copy the file `tick.lua` to our project

2. we need to `require` `tick.lua`
```lua
function love.load()
	tick = require "tick"
end
```

* notice `require` doesn't have parantheses `()`
	* they can have
	* but when we only pass 1 argument, we don't need it
* with `require` it's common to leave them out
	* althought that is not the case for any other function

3. update `tick`
```lua
function love.update(dt)
	tick.update(dt)
end
```

now we're ready to start using the tick library
let's make it so a rectangle will be drawn after 2 seconds

```lua
function love.load()
	tick = require "tick"
	
	drawRectangle = false
	
	-- first argument is a function
	-- second argument is how much time to wait before calling the function
	tick.delay(function () drawRectangle = true end, 2)
end

function love.draw()
	if drawRectangle then
		love.graphics.rectangle("fill", 100, 100, 300, 200)
	end
end
```

### Standard Libraries
Lua has built-in libraries, called *Standard Libraries*.
* functions that are built into Lua
* eg. `print()`, `table.insert()`, `table.remove()`

*math library* is one of them
* useful when making a game
* eg. `math.random()` gives us a random number

```lua
function love.load()
	x = 30
	y = 50
end

function love.draw()
	love.graphics.rectangle("line", x, y, 100, 100)
end

function love.keypressed(key)
	if key == "space" then
		x = math.random(100, 500)
		y = math.random(100, 500)
	end
end
```
___

# Classes
* we need the [rxi/classic: Tiny class module for Lua (github.com)](https://github.com/rxi/classic) library for that

1. copy `classic.lua` to our project
2. `require` it
```lua
--! file: main.lua
function love.load()
	Object = require "classic"
end
```

3. to make a class, create a new file called `rectangle.lua`
```lua
--! file: rectangle.lua

-- create a new class, named Rectangle
Rectangle = Object.extend(Object)

function Rectangle.new(self)
	self.test = math.random(1, 1000)
end
```
[^Rectangle]: btw, camelCase but upper-cased is called *UpperCamelCase* or *PascalCase*

4. then we can use `Rectangle` in `main.lua`
```lua
--! file: main.lua
function love.load()
	Object = require "classic"
	require "rectangle"
	
	r1 = Rectangle()
	r2 = Rectangle()
	print(r1.test, r2.test)
```

`r1` and `r2` are unique
* by calling `Rectangle()` it'd create a new instance
	* instance is a new object with all the class features
* every new instance is unique
	* `r1` and `r2` both have a property `test`, and they have different values

by calling `Rectangle()` it'll execute `Rectangle.new(self)`
* it's called the *constructor*
* `self` is the instance
	* that we're modifying

if we were to type `Rectangle.test = math.random(0, 1000)`
* it'd be giving the property to the class, but not to an instance

5. let's give some more functions to the class `Rectangle`
```lua
--! file: rectangle.lua
Rectangle = Object.extend(Object)

function Rectangle.new(self)
	self.x = 100
	self.y = 100
	self.width = 200
	self.height = 150
	self.speed = 100
end

function Rectangle.update(self, dt)
	self.x = self.x + self.speed * dt
end

function Rectangle.draw(self)
	love.graphics.rectangle("line", self.x, self.y, self.width, self.height)
end
```

6. then in `main.lua`, the code for drawing a rectangle is much cleaner
```lua
--! file: main.lua

function love.load()
	Object = require "classic"
	require "rectangle"
	r1 = Rectangle()
end

function love.update(dt)
	r1.update(r1, dt)
end

function love.draw()
	r1.draw(r1)
end
```

7. it's annoying to pass `r1` to itself as an argument every time we call its functions though
```lua
--! file: main.lua
function love.update(dt)
	r1:update(dt)
end

function love.draw()
	r1:draw()
end
```

* this way Lua will automatically pass the object left of the colon `:` as the function's first argument
* similarly we can do this with the function declaration as well
```lua
--! file: rectangle.lua

Rectangle = Object:extend()

function Rectangle:new()
	self.x = 100
	self.y = 100
	self.width = 200
	self.height = 150
	self.speed = 100
end

function Rectangle:update()
	self.x = self.x + self.speed * dt
end

function Rectangle:draw()
	love.graphics.rectangle("line", self.x, self.y, self.width, self.height)
end
```

* shorthands like these are called *Syntactic sugar*

8. finally let's improve `Rectangle:new()` a bit

```lua
--! file: rectangle.lua
function Rectangle:new(x, y, width, height)
	self.x = x
	self.y = y
	self.width = width
	self.height = height
	self.speed = 100
end
```

```lua
--! file: main.lua
function love.load()
	Object = require "classic"
	require "rectangle"
	r1 = Rectangle(100, 100, 200, 50)
	r2 = Rectangle(350, 80, 25, 140)
end

function love.update(dt)
	r1:update(dt)
	r2:update(dt)
end

function love.draw()
	r1:draw()
	r2:draw()
end
```
___

# Inheritance
with inheritance, we can extend our class
* make a copy of a class, then add features to it
* without editing the original class

![extension.png (643×311) (sheepolution.com)](https://sheepolution.com/images/book/11/extension.png)

eg. making monsters
* every monster has their own attack and move, but differently
* every monster gets damage, and dies, in the same way
* the overlapping features should be put in what we call a *superclass* or *base class*
	* they provide features that all monsters have
	* then each monster's class can extend this base class and add their own features

* base class or superclass are just a term, they have no difference than any other class
	* just the way that we gonna use these classes are different

let's create another moving shape, a circle, besdies rectangle
* both of them can move, just different shape

1. first we make a base class, `Shape`
```lua
--! file: shape.lua
Shape = Object:extend()

function Shape:new(x, y)
	self.x = x
	self.y = y
	self.speed = 100
end

function Shape:update(dt)
	self. x = self.x + self.speed * dt
end
```

2. now `Rectangle` should be an extension of `Shape`
```lua
--! file: rectangle.lua

-- make Rectangle an extension of Shape
Rectangle = Shape:extend()

-- override Shape:new()
function Rectangle:new(x, y, width, height)
	Rectangle.super.new(self, x, y)
	self.width = width
	self.height = height
end

function Rectangle:draw()
	love.graphics.rectangle("line", self.x, self.y, self.width, self.height)
end
```

`super` means the class that this one is extended from
* in our case, `Rectangle.super` is `Shape`
* `Rectangle.super.new(self, x, y)` means `Shape:new(x, y)`
	* although the two is not exactly the same
	* can't use colon `:` because we're not calling the function as the instance
		* `self` should be `Rectangle`, of this instance
		* instead of `Shape`

`self` means this instance of the class

3. and now another class for `Circle`
```lua
--! file: circle.lua
Circle = Shape:extend()

function Circle:new(x, y, radius)
	Circle.super.new(self, x, y)
	self.radius = radius
end

function Circle:draw()
	love.graphics.circle("line", self.x, self.y, self.radiu)
end
```

4. to use them in `main.lua`
```lua
--! file: main.lua

function love.load()
	Object = require "classic"
	require "shape"
	require "rectangle"
	require "circle"
	
	r1 = Rectangle(100, 100, 200, 50)
	r2 = Circle(350, 80, 40)
end
```
___
