[Sheepolution - How to LÖVE](https://sheepolution.com/learn/book/contents)
___

# Why LÖVE?
* also known as Love2D

check out other tools instead for a single simple game
* if you don't want to touch game development thereafter
* Construct 2 or Game Maker

It's beginner friendly in terms of programming
but provides the functionality to make professional 2D games

It's a framework, not an engine
* Game Maker, Unity are game engines
	* provides built in level editor and other tools
* we do everything with code

Benefit:
* we will understand exactly what is going on and how it works
	* because we wrote it
* the knowledge is easier to transfer to other engines and frameworks
___

# Lua syntax - intro

##### comments
```lua
-- this is a comment
print(123)
--Output: 123
```

* `print()` messages show up in editor **when we close the LÖVE program**
	* if we want to print the message right away we need
	* `io.stdout:setvbuf("no")` at the top of the code
		* it turns off that the program should wait to be closed, before showing the prints
* `love.graphics.print()` shows up in LÖVE program

##### variables
```lua
a = 5
b = 3
print(a + b)
--Output: 8
```

* case-sensitive
	* `sheep`, `SHEEP`, `sHeEp` are 3 different variables
* can't start with number
* can't include any special characters like `@#$%^&*`
* can't be a reserved keyword

##### math operators
```lua
a = 20 + 10 -- add
b = 20 - 10 -- substract
c = 20 * 10 -- multiply
d = 20 / 10 -- divide
e = 20 ^ 10 -- exponentiate
```

##### data type
```lua
a = 10         -- integer
pi = 3.141592  -- float
text = "Hello" -- string
```

##### string concatenate
```lua
name = "Daniel"
age = "25"
text = "Hello, my name is " .. name .. ", and I'm " .. age .. " years old."
print(text)
--Output: "Hello, my name is Daniel, and I'm 25 years old."
```

##### functions
```lua
-- 1.
example = function()
	print("Hello World!")
end

-- 2.
function example()
	print("Hello World!")
end

-- execute the function
example()
--Output: "Hello World!"
```

###### parameters
```lua
function sayNumber(num)
	print("I like the number " .. num)
end

sayNumber(15)
sayNumber(2)
sayNumber(44)
print(num)
--Output:
--"I like the number 15"
--"I like the number 2"
--"I like the number 44"
--nil
```

* little knowledge: `num` is *parameter*; `15`, `2`, `44` are *arguments*
* `nil` means no value
	* not a number, or string, or functio
	* it's nothing

###### return
```lua
function giveMeFice()
	return 5
end

a = giveMeFive()
print(a)
--Output: 5
```

> [!important]
> Don't repeat yourself!

___

# What is LÖVE?
it's a framework
* a tool to make programming games easier

made with C++ and OpenGL
* both are very difficult
* so source code of LÖVE is indeed very complex

eg. source code of `love.graphics.ellipse()` is
```c++
void Graphics::ellipse(DrawMode mode, float x, float y, float a, float b, int points)
{
	float two_pi = static_cast<float>(LOVE_M_PI * 2);
	if (points <= 0) points = 1;
	float angle_shift = (two_pi / points);
	float phi = .0f;
	
	float *coords = new float[2 * (points + 1)];
	for (int i = 0 ; i < points; ++i, phi += angle_shift)
	{
		coords[2*i+0] = x + a * cosf(phi);
		coords[2*i+1] = y + b *sinf(phi);
	}
	
	coords[2*points+0] = coords[0];
	coords[2*points+1] = coords[1];
	
	polygon(mode, coords, (points + 1) * 2);
	
	delete[] coords;
}
```

### so what's Lua?
a programming language, that LÖVE uses

not made by or for LÖVE
* creators of LÖVE simply choose Lua for their framework

everything starting with `love`, is part of the LÖVE framework


### How does LÖVE work?
it calls 3 functions

1. `love.load()`
* we create our variables here
* called once when the program first started

2. `love.update()`
3. `love.draw()`
* repeatedly call this 2 functions

* these functions along with the code that we put inside of those, are called *callback*

LÖVE is made out of *modules*
* `love.graphics`, `love.audio`, `love.filesystem`, etc.
* there are about 15 modules, each focuses on 1 thing

Use [LOVE wiki](https://www.love2d.org/wiki/Main_Page) to learn about the modules and their functions
* eg. `love.graphics.rectangle()`
	* `DrawMode` is either `"fill"` or `"line"`
* the functions that LÖVE provides is what we call the API, *Application Programming Interface*
___
