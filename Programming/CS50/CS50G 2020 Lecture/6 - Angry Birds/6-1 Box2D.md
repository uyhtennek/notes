# Angry Birds

* released in 2009
* sling shot birds into some structure and knock it down
	* the whole entire underlying mechanism - physics engine
* Box2D
	* probably the most ubiquitous 2D physics engine
___

# Spritesheet with no tile

This time our arts have organic shapes
* many of those are not the same size

We have to hard code the different x, y, width, height to do the quads
* quite tedious, but not really any other solution
___

# Box2D Readings

[love.physics - LOVE (love2d.org)](https://love2d.org/wiki/love.physics)

[Tutorial:Physics - LOVE (love2d.org)](https://love2d.org/wiki/Tutorial:Physics)
* a simple tutorial to make a ball bounce
* using Box2D

[Introduction - Box2D tutorials - iforce2d](http://www.iforce2d.net/b2dtut/introduction)
* this is how the teacher learnt Box2D
* very detailed and completed
	* eg. making tanks and pulleys and more
* worth a look if you're making a physics based game

Box2D is a library
* written in C++
* we can plug-in pretty much anywhere we want to
* most 2D game engines use it
	* Live GDX, a very large Java 2D game framework
	* Unity uses it as well
___

# The World

Core of any Box2D app or game, is the **world** object
* all the physics calculations are taken care by it
* if we want pieces to interact with each other in Box2D, we'd need to put them all in the world
	* these pieces are what Box2D called *fixtures* and *bodies*

The world update all of the physics things
* apply the relevant forces and do the physics calculations
* maybe to resolve collisions, and all sorts of things

We don't manually go through and update every single object
* like all the dx, dy, gravity, and checking for collisions that we've done before
* the world takes care of all of them, based on how we told it to resolve them

Also, applying gravity being one of its main things
* can be either or both x- and y-axis
* going down = positive value in y-axis
	* 300 is the number in our example

`love.physics.newWorld(gravX, gravY, [sleep])`
* `love.physics` encapsulates all of the Box2D functions and objects
	* that love2D can use
	* in short, `love.physics` is a wrapper for Box2D
		* basically people that created love2D decided to use Box2D
		* and put a bunch of Lua functions around the Box2D things, so that we can use it as the same style that we used love2D
* optional `sleep` parameter, to allow non-moving Bodies in our world to sleep
	* so it tells Box2D to not calculate physics for them
___

# Bodies

* it's just an abstract container
	* holds a position and a velocity
* and we attach things to it, via *fixtures*
	* it gives the Body a shape, and therefore a collision box
* and therefore allow it to do physics interactions

So Bodies are essentially all the things in our scene, that they can move around and collide or interact with things, ie. other Bodies

`love.physics.newBody(world, x, y, type)`
* when ever we create a new Body, we pass in the `world`, so the World has access to all the Bodies
	* then every time we do update the World, it knows to update all the Bodies
* we also pass in `x`, `y`, so we place it in the world on instantiation
* there are 3 foundamental `type` of Bodies
	* `'static'`, `'dynamic'`, `'kinematic'`, that we'll later discuss
	* in short, it influences how the Body will interact with other bodies
___

# Fixtures

* it's an abstract object, that will allow us to attach a shape to a Body
	* Bodies are shapeless by default
	* Bodies are just containers, with position and velocity, effectively
		* don't interact and don't know how to interact with others, untill we give them a fixture
* thus influence how things or Bodies interact. influence collisions

In addition to attaching shape to Bodies, it can attach some more other things
* density, things with higher density have more weight, more mass, and will push things farther when there is collision
* friction
* restitution, = bounciness
	* when something with no restitution falls and hits the ground, it'll just fall flat
	* if we give it a restitution then it'll bounce when it hits the ground

there are some default shapes given in love 2D
* `love.physics.newCircleShape(radius)`
* `love.physics.newRectangleShape(width, height)`
* `love.physics.newEdgeShape(x, y, width, height)`
* `love.physics.newChainShape(loop, x1, y1, x2, ...)`
* `love.physics.newPolygonShape(x1, y1, x2, y2, ...)`
	* this one can define arbitrarily any shape, maybe like a pentagon

`love.physics.newFixture(body, shape)`
* creates a new Fixture for the `body`
* apply the `shape` to the `body`
	* to the `body`'s center
* after that, the World can calculate how it collides
___

# Body Types

`'static'`
* the Body will exist in the world
* but not affected by gravity or collision or anything else
	* cannot be moved as by applying force
* things can hit it and bounce off of it, and do their own things, though
	* just the static body will never be influenced by something else

* so in short, it just doesn't move. some sort of permanent structure.
* something like the ground
	* we don't really affect the ground by colliding with it
	* except in real life we do can affect the ground though, with enouth force


`'dynamic'`
* opposite to `'static'`
* has the full simulation of Box2D
	* affected by gravity, collisions, etc.

* do everything that we'd expect a "normal" body would do
* eg. a ball, can hit the wall and bounce off
	* the wall in this case would be the static body


`'kinematic'`
* hybrid between the above two
* it can move and do its own things
* but not influenced by other objects

* suitable for certain environmental pieces
* eg. an indefinitely rotating platforms
	* we programmed it to spin to infinity
	* not affected by gravity, and doesn't slow down or speed up when something hits it

how about kinematic-to-kinematic interactions?
* the bodies will just pretend each other doesn't exist and continue to do their own things
___

# Example Code

## static body

```lua
function love.load()
	world = love.physics.newWorld(0, 300) -- 300 downwards gravity force
	boxBody = love.physics.newBody(world, VIRTUAL_WIDTH / 2, VIRTUAL_HEIGHT / 2, 'static') -- Box2D defines everything by its center point, instead of its top left
	boxShape = love.physics.newRectangleShape(10, 10)
	boxFixture = love.physics.newFixture(boxBody, boxShape)
end

function love.update(dt)
	world:update(dt)
end

function love.draw()
	love.graphics.polygon('fill', boxBody:getWorldPoints(boxShape:getPoints()))
end
```

`boxBody:getWorldPoints()`
* a Box2D function given to any Body
* gets where it is in the world, along with all of its vertices
* `boxShape:getPoints()` gets the vertices of the shape

we didn't use `love.graphics.rectangle()`
* because we can't
* `getWorldPoints()` can explodes out to `love.graphics.polygon()`
	* but not necessarily `love.graphics.rectangle()`, and in fact, it doesn't
	* the output match the number of arguments that would satisfy `love.graphics.polygon()`


## dynamic bodies
and a ground

```lua
function love.load()
	world = love.physics.newWorld(0, 300)
	
	-- dynamic box
	boxBody = love.physics.newBody(world, VIRTUAL_WIDTH / 2, VIRTUAL_HEIGHT / 2, 'dynamic')
	boxShape = love.physics.newRectangleShape(10, 10)
	
	boxFixture = love.physics.newFixture(boxBody, boxShape)
	boxFixture:setRestitution(0.5)
	
	-- ground
	groundBody = love.physics.newBody(world, 0, VIRTUAL_HEIGHT - 30, 'static')
	edgeShape = love.physics.newEdgeShape(0, 0, VIRTUAL_WIDTH, 0) -- edge = a line
	groundFixture = love.physics.newFixture(groundBody, edgeShape)
end

function love.update(dt)
	world:update(dt)
end

function love.draw()
	-- draw box
	love.graphics.setColor(0, 255, 0, 255)
	love.graphics.polygon('fill', boxBody:getWorldPoints(boxShape:getPoints()))
	
	-- draw ground, a line
	love.graphics.setColor(255, 0, 0, 255)
	love.graphics.setLineWidth(2)
	love.graphics.line(groundBody:getWorldPoints(edgeShape:getPoints()))
end
```

`edgeShape = love.physics.newEdgeShape(0, 0, VIRTUAL_WIDTH, 0)`
* notice we passed in x, y, width, height, as `(0, 0, VIRTUAL_WIDTH, 0)`
* but it's not where the shape will be drawn
	* where the shape will be drawn is relative to the body's location
* so it's an edge with a `(0, 0)` offset from the center of the Body, and with the width and height `(VIRTUAL_WIDTH, 0)`


## kinematic bodies

```lua
function love.load()
	...
	kinematicBodies = {}
	kinematicFixtures = {}
	-- we just need one shape and can apply it to any number of Bodies
	kinematicShape = love.physics.newRectangleShape(20, 20)
	
	for i = 1, 3 do
		table.insert(kinematicBodies, love.physics.newBody(world,
			VIRTUAL_WIDTH / 2 - (30 * (2 - i)), VIRTUAL_HEIGHT / 2 + 45, 'kinematic'))
		table.insert(kinematicFixtures, love.physics.newFixture(kinematicBodies[i], kinematicShape))
		
		-- spin the body
		kinematicBodies[i]:setAngularVelocity(360 * DEGREES_TO_RADIANS)
	end
end

function love.update(dt)
	world:update(dt)
end

function love.draw()
	...
	love.graphics.setColor(0, 0, 255, 255)
	for i = 1, 3 do
		love.graphics.polygon('fill', kinematicBodies[i]:getWorldPoints(kinematicShape:getPoints()))
	end
end
```

`kinematicBodies[i]:setAngularVelocity(360 * DEGREES_TO_RADIANS)`
* Box2D expects radians when it comes to rotation
* recall $\pi \space radian = 180\degree$
___

# more

## joint

usually big or complex physics structures involve *joints*
* it joins Bodies together
* eg. wheels of a car are jointed to the car, so that when ever the wheels spin, it moves the car
* eg. a bridge is many board jointed together


## screen box

if we want things to be inside of the screen at all time
remember to add, besides the ground, 2 vertical walls at the left and right sides of the screen
* also we don't have to render them


## density

remember Bodies with higher density, push Bodies with lower
* eg. if we want a square to sink in a pool of balls, the square needs to have a higher density


> [!tip]
> Physics is fun.

___
