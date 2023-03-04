# Helicopter Game

* famous in the 2000s
* a web game, a flash game
	* Addictinggames.com still has it on there

* old precursor to Flappy Bird
	* avoid hitting on the ceiling and the floor and the obstacles
	* click to go up

> [!2.5D Game]
> Although everything is in 3D, the actual axes upon which we're bound are just two.
> We can only move along X and Y axes, but all the models are in 3D.

___

# Unity

Now we're actually using a full-fledged game engine, a system
* have a built in editor
* have all the cool functionality that we didn't get with Love2D

We were using a framework called Love2D
* then just implement everything purely in code

Unity has a tremendous market share
* in 2016 it had like 43% of all games released were done in Unity
* Unreal actually may have more market share now
	* because of games like Fortnite
	* also improved a lot over the last couple of years, and marketed well
* others are Godot, CryEngine, etc.

* but Unity being a very easy engine to start with
* to crank things out and be productive with
	* other engines like Unreal might have a deep learning curve
		* it is more user-friendly though, but we need to learn C++ to get down into the nitty gritty with Unreal
* and Unity is free
	* although Unreal, Godot, and some others are free as well
	* until we start making over $100,000 in gross revenue releasing Unity products
		* the next tier is $200,000 and then we can get new features

If you want to just start up a new company, and use Unity to take something to market,
it's completely free
* by when you can gain $100,000, it's not too much to ask to start paying Unity

In mobile and VR, Unity is sort of like the forefront
* market share got even higher percentage
	* 60-something or 70%
* it marketed itself very strong towards the use of VR

Lastly, we use C# to get things implemented
* very different to Lua
	* which is a dynamic scripting language, very much like to JavaScript
* C# is very similar to Java, in which things have types
___

# C\#

* Statically-typed object-oriented language
* created by Microsoft
	* initially to compete with Java

there are ways to run C# code on other platforms, primarily with the use of what's called Mono
* an open source implementation of what's called CLR, *common language runtime*
* this is how Microsoft allows several of its language, like F#, C#, Visual C++, Visual Basic, to all compile to this intermediary format
	* like how Java compiles to bytecode
	* so this is a layer between programming language and assembly language
* we can run any of those languages' code with the CLR

Mono is also how Unity allows its use across multiple operating systems
* Unity uses a CLR, Mono, to run its program
* that is not just Windows specific, but can also run on Mac and on Linux machines as well
* we can see evidence of its existence via the nomenclature used for MonoBehaviour

From the beginning, Unity was actually developed for Mac platforms, exclusively
* in 2011 or 2012 they ended up making it usable across all major desktop operating systems


## C\# syntax

Unlike Lua, which we can create variables by typing `x = 10` or `tbl = {}`
* In C# we have to specify the data type, so we need to say `int x = 10`
* so there's more syntax to be conscious of
	* but we trade off this dynamism for more performance
	* ultimately this is what it comes down to static versus dynamic languages
		* performance verse flexibility
* so C# is a lot faster than Lua

* in fact, Unity itself is not programmed in C#, but C++
	* but it allows us to program any behavior that we want in C#

C# is an object-oriented programming language
* there are objects that are *public* and *private*
	* so we have access specifiers for variables and classes
	* that tell other variables and classes whether or not they can communicate
* public class means other classes can see this class
* widely used even outside of Unity for games
	* eg. GUI applications, .NET applications, and Mono applications
___

# Blender
a tool to create new assets
[blender.org](https://www.blender.org/)

technically Blender does have a game engine built-in too
* but it's not used for making games very much
* but to create new 3D models

Completely free
Open-source
___

# Game Object

a game object is basically anything in Unity
* camera is a game object
* Helicopter is a game object
* Direction Light, game object
* SkyscraperSpawner, we don't see it anywhere, but that's also a game object
	* an invisible one


## Canvas

Unity does an interesting thing with all of its GUI elements
* it maps the canvas to, 1 pixel = 1 Unity unit
	* 1 Unity unit is usually a meter
* it draws the canvas in a different draw call than it draws all of the actual 3D stuff

So when you create a canvas and you see it's gigantic
* that's just Unity's way of doing GUI
* although it's strange, but it's for performance reasons
	* they don't need to calculate where the canvas and UI is in the world space
	* otherwise calculating ration and working with the fractional points would be bad


## Game Object = Entity

Game Objects in Unity, are just containers
* the beginning of an Entity Component System
* they are the Entity class, or container class, that drives behavior
___

# Components

so Game Object is just a container, but for what? Components.
and it's components that can drive behavior

Components are, all of the things, in the Inspector window in Unity Editor

if we create a new Game Object, we can see its only component is `Transform`
* with 3 fields with children, `Position`, `Rotation`, `Scale`

* and we can modify them at will
	* eg. we moved `Position > X`
* Unity knows to look at the transform component and render the object based upon the transform position


## Camera

`Camera` is also a component
* we can attach `Camera` to any Game Object, and it becomes a camera
	* then maybe we want to set it to be the default camera
* we have access to the fields that are relevant to how the camera does its job, just in Unity Editor
	* we don't even need to touch code, for a lot of things

`Camera` has 2 `Projection`, = 2 modes
* Perspective projection. It models how real cameras, real lenses, human eyes, etc. behave.
	* things distort a little bit
	* they look wider or skewed, not completely geometrical, have vanishing points
* Orthographic project. Everything is completely flat, no matter how far away the object is and in what angle we look at it.

Just a click of a button, we can change how our game looks completely
* eg. Crossy Road does everything with a orthographic camera
* eg. we can change Field Of View to see more of the game world
	* in Quake and other shooters, we can change FOV, usually in the menu settings
	* so we can see more around us
		* at some point it can look a little bit like a fisheye lens
		* how it's done is just more distortion than the perspective projection

> [!tip]
> Generally we don't have to mess with the fields too much. Usually we'll end up finding specific areas of the editor and specific areas of the code base and documentation that we dive deeply into.

The Camera game object also has an `AudioListener` to listen for audio, and an `AudioSource` to play a sound or music
* so it does listen to itself


## Uses of Components

1. create or combine behaviors, for our game objects
2. Unity allows us to modifty components, in the editor itself
	* depending on how we implement or design them
	* if we modeled the components well, we don't need to touch the code to whip things up

___
