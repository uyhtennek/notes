[Making Small-Scale 2D Games with LÖVE 2D and Lua - CS50 Seminars 2021](https://video.cs50.io/iOA5YspoJDM)
___

# What is Scope?
the extent of a player's possible in-game activity

* which is going to create certain restrictions upon the person who is making the game
* as well as certain demands

Think of the games that you enjoy right now.
Is it a small tight game, which has a play time of minutes?
Or a big blockbuster games, which has a play time of hours, and took many people to develop?

> [!quote]
> You see, the most common and persistent mistake that game designers can make, is overcommitment to the scope of the game.

those overcommitments often come in terms of
* the features that are in the game
* the artwork of the game
* the storyline

remember that we're on some sort of time frame
> [!quote]
> You want to do useful things within a useful time period.

so how much time are you willing to invest in this game project?
* 2 weeks? 3 weeks? 4 weeks?
* many months?

if this is your first one, choose a small time frame
14 days, or maybe 10 days at most
___

# Prevent Overcommitment
how not to overcommit to a scope that's going to exceed our capabilities and our available time?

the worst thing to happen would be,
we get into a project, but sometimes later we give up and walk away
* it would be like a cut to oneself
* in the way that it's developing a habit of not finishing

so how to prevent it?


1. Take stock of what game dynamic is at the core of your desire to create a game

* game dynamic = the basic gameplay of a game
	* what makes a game, a game
	* what makes a game, fun
	* eg. throwing a ball at a target, putting a ball in a hole, keeping a ball out of a hole, touching your opponent, running away from your opponent, hiding from or finding your opponent, avoiding obstacles amongst many others, etc.
* so there is some sort of core game dynamic in every game

**10 core game dynamics**
by *Romero & Schreiber, 2017, pp.5-8*
1. Territorial Acquisition;
2. Prediction;
3. Spatial Reasoning;
4. Survival;
5. Destruction;
6. Building;
7. Collection;
8. Chasing or Evading;
9. Trading; and
10. Race to the End

eg. Mario
* avoid being touched
	* think of Mario's first level
		* the bricks, the coin box can be touched, but the Goomba can't, because it'll kill Mario
	* it sets up a game dynamic that there are objects that we can touch and they are OK, but there are other objects that we can touch but they are not OK
	* it bascially is an obstacle course

* in a way, it is a race to the end game, where the dynamic is don't be touched
	* if the dynamic is destruction, it is not Mario but maybe My Friend Pedro

so what's the game dynamic of your game gonna be?


2. Take stock of the game that you would like to create

* maybe think about it in terms of game features
* it's kinda dangerous because it's tempting to load up on features
	* we could easily overcommit to the number of features

so remember what's the core game dynamic of the game
* and think of what are the features that are gonna be in our game that support its core dynamic
* what are the bare number of features that we would need to accomplish the core game dynamic
* how do you win the game? how do you play it?
	* that the start point of a very tight game

Here are some features that we need to pay attention to:
* The player
	* who is the player?
* Environment
	* how the player move?
* Characters
* Artwork
	* often programmers can get distracted in artwork
	* sort of live there, instead of working on the core dynamic of the game
* Abilities
* Levels
* Power-ups
* Scoring

These are pieces about what makes the game unique

eg. Endless Sky
* a open-source game, available on Steam
* player is in their own shuttle, floating in a seemingly endless star system, alongside other NPC ships that are attacking them, stealing items from them, engaging in trade with them
* drawn with this high-density, professional art
* every ship is able to expand their capabilities using thousands of different power ups
* players get to decide where in this universe to travel
	* to explore hundreds and hundreds of star systems searching for power-ups
* and ultimately build the ultimate fleet of our own

now you understand how big this game project is
now imagine how confusing it could be if it wasn't reigning in each one of those features
* yeah we have a great game idea in our mind
* but do we have the time and resources to make that happen?

think of Mario again
if we need to make Mario wihtin 2 weeks, what we need to accomplish that?

* probably first step is we would limit the game down to just 1 level
	* maybe not even an 1 whole level, but really just 1 frame
	* one little piece of that giant level
* that way we can get the core game dynamic down and get it working, before we expand out to something greater if we have time

* maybe instead of making all kinds of power-ups for small Mario, big Mario, fire Mario, etc.
* we just need small Mario

* same goes for the enemies, we don't need all of them
* we just need Goomba

at the end of the day,
what can present our game dynamic well, in a 2-week or so time period?

> [!quote]
> A finished prototype of a game that we potentially amp up into something greater is far more effective than an unfinished, massive game that we attempted but never finish.


3. Reduce the game down to a Minimal Viable Product, a MVP

* how do we represent Mario in the minimal way possible?
* Mario, Goomba, bricks
	* player's character, things that are not ok to touch, things that are ok to touch

so figure out what are the
* must-have features
	* the things that without them our game doesn't exist
* nice-to-have features
	* the things that could create a better game, but are not the essential core dynamic
	* even later when we start implementing these features
		* try not to stuck at things that we don't need to address at the moment
		* artworks being one of them
___

# Game Engine or Frameworks

back in the days,
hardwares are expensive, and RAM are very limited

people in that era created games written in maybe C
* eg. DOOM, Wolfenstein
* it's one of the few options
* people need to get as low level as they can get, to make the most out of the very limited and very expensive hardware

but now, since game programming is more popular than it used to be
game frameworks started to show up
* Game Creator, no need to code at all
* Unity, usable by most people
	* also very graphical, and offers many different things
* LÖVE 2D
* Adventure Construction Set
* GameMaker Studio, Godot, Corona SDK, etc.
___

# LÖVE Basics
need 3 main functions to make a game run

`function love.load()`
* runs only once, once the file is loaded

`function love.update()`
* runs every frame when we're running the program

`function love.draw()`
* draws something
* eg. `love.graphics.rectangle("line", 100, 100, 200, 200)`
	* `width` and `height` are `100` pixels; position is, from the top left, `200` over and `200` down

eg. 1 - just draw a square
```lua
function love.load()

end

function love.update()

end

function love.draw()
	love.graphics.rectangle("line", 100, 100, 200, 200)
end
```

eg. 2 - move the square
```lua
function love.load()
	x = 100
	y = 100
end

function love.update()
	
	if love.keyboard.isDown('w') then
		y = y - 1
	elseif love.keyboard.isDown('s') then
		y = y + 1
	elseif love.keyboard.isDown('a') then
		x = x - 1
	elseif love.keyboard.isDown('d') then
		x = x + 1
	end
	
end

function love.draw()
	love.graphics.rectangle("line", x, y, 200, 200)
end
```
* but, the square can move out of the screen

eg. 3 - introduce collision
```lua
function love.load()
	mySquare = {}
	mySquare.x = 100
	mySquare.y = 100
	mySquare.height = 200
	mySquare.width = 200
end

function love.update()
	
	if love.keyboard.isDown('w') then
		mySquare.y = mySquare.y - 1
	elseif love.keyboard.isDown('s') then
		mySquare.y = mySquare.y + 1
	elseif love.keyboard.isDown('a') then
		mySquare.x = mySquare.x - 1
	elseif love.keyboard.isDown('d') then
		mySquare.x = mySquare.x + 1
	end
	
	if mySquare.y < 0 then
		mySquare.y = mySquare.y + 4
	elseif mySquare.y > 400 then
		mySquare.y = mySquare.y - 4
	elseif mySquare.x < 0 then
		mySquare.x = mySquare.x + 4
	elseif mySquare.x > 600 then
		mySquare.x = mySquare.x - 4
	end
	
end

function love.draw()
	love.graphics.rectangle("line", mySquare.x, mySquare.y, mySquare.width, mySquare.height)
end
```
___

# Phases of game development

1. Setup
* get the tools, the game engine, the framework running

2. Gut-Check
* Do I want to use this tool? Is it the right tool to make my game?
* it's just a matter of an hour or two to make this decision

3. Game Dynamic
* make the game at least work
* maybe show it to someone, have someone to play it and see how it goes

4. MVP
* complete the game
	* has a beginning and an ending
	* or has the potential of being perpetual forever
* make it releasable

5. Feature
* make it an even better game
* what we're doing here are on top of what has already been deemed eseential
___

# Starter Resources

[Sheepolution - How to LÖVE](https://sheepolution.com/learn/book/contents)
* very comprehensive, step-by-step tutorial
* it get to a built game very quickly
* [[How to LÖVE - 1 LÖVE and Lua]]

[Simple Game Tutorials for Lua and LÖVE](https://simplegametutorials.github.io/love/)
* reimplement games that we already know about
* eg. Pong

Brenda Romero and Ian Schreiber
"Challenges for Game Designers"
* both authors also have other books about the game industry which are exceptionally good
* but this one to start with

[Extra Credits](https://www.extracredits.site/)
* a YouTube channel

[CS50's Introduction to Game Development (harvard.edu)](https://cs50.harvard.edu/games/2018/)
___

# 3 Landmines for small games

1. skipping prototyping
* make a prototype first to see if the core game dynamic works or not
	* "taking a stone and dipping it in chocolate, is not gonna make it taste better"
	* likewise, adding 16 levels to a bad game, is still a bad game
* often times developers get so far down the production pipeline, to a point that they don't want to turn from this bad idea, because they've invested so much into it
	* so prototype first to see if it's a good direction or bad, before investing

2. over-scoping
* the game is simple too large
	* beyond our capability
	* but it's more about our time really
		* beause we can always outsource creativity, so capability isn't really an issue
		* just find someone who is capable
* if the game is over-scoped in terms of time, that's for sure is a big deal

3. focusing on features over function
* sometimes developers try to add features that get away from the core game dynamic
* focus on the minimal viable, the core dynamic of the game, before we go and create a million features on top of that core dynamic
___
