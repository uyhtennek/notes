[Juice it or lose it - a talk by Martin Jonasson & Petri Purho](https://www.youtube.com/watch?v=Fy0aCDmgnxg)
___

# What is Juiciness?
Things that wobble, squirt, bounce around, or make cute noises.
Things that feel good to interact with.

The term "juicy" comes from a classic Gamasutra article, 'How to prototype a game in under 7 days'
> [!quote]
> A juicy game feels alive and responds to everything you do, tons of cascading action and response for minimal user input, typically visual or auditory, but it doesn't really need to be.

Emily Short mentions on her blog that, although interactive fiction can't really do wiggles and squirts much, they have their own kind of juice
* the fun, and sort of unexpected responses
* that we can get when we try something, anything

> [!quote]
> Juice is about, maximum output for minimum input.

## Why juiciness?
it makes our games better, always, guaranteed
___

# How to juice?
eg. making a Breakout clone


## Color
* bad guys are red, bullets are yellow, ....
* not just all white


## Tweening
* also known as Easing
* the word comes from "in-betweening"

```c#
t = EasingFunction(t);
float new_value = start_value + (t * (end_value - start_value));
```

`x += (target -x) * .1;`
* there are times that we don't have a tweening engine, or we are lazy, or something, but we can always use this formula
* `x` first goes fast, then slow down as it get closer to `target`
	* `x` increments by 10% of the distance between `target` and `x` for each iteration

maybe tween the opening of the game
* have the game fall off and bounce back for a little while before actually starting the game
* tween the rotation and orientation of some stuffs to make them look interesting

### Delay
a beautiful combo to use is that, adding some delay or random delay to all the things
* so that everything tweens in different start time and end time


## Scale
or a better term, squeezing and stretching

when something is moving fast, it looks funnier if we squeeze and stretch it
* sometimes depend on the shape of the object, it can look better sometimes than the other times

similarly when something hits something, we can make it bigger than usual for a bit
* we can make it wobble for a bit too, after the hit took place
* we can do the same for the thing that is being hit

### Blink Color
a common combo to use when something get hit is that, it blinks
* maybe to white

### Global effect
another combo for hitting something is, the whole get affected some how
* eg. every other objects scale like jelly for a bit
* eg. camera shake


## Sound
eg. a demo from the book 'Game Feel' by Steve Swink
* two circles that pass through each other, that's it

add a "bolk" sound to it, like the sound of woods colliding with each other
* it feels like the circles are bouncing off each other instead of passing through

Every interaction, should have sounds
And let's not forget the music
___

# Particles

## classic smoke
* you can make your own or just use whatever you find

## left over
* when something die, it doesn't just disappear
* eg. in Breakout, when a block is hit
	* push it away from its location and fall off due to gravity, almost like throwing it
	* make it darker so it is clear that it dies

## Shatter effect
* when something breaks, it breaks into more parts
* 1 whole thing breaks into 2 parts, usually will do
* or, surely we can make use of particle effect for creating shatters

## Trail
just looks very fancy
___

# More

## Screen Shake
no need to explain
* just be careful to not add too much

## Eyes
* a classic Kyle Gabler trick
* just, add eyes to things that we have
	* can make it look at something to make it nicer
	* btw, bigger eyes are usually better
* same goes for adding a mouth
* it is good because it adds insane personality to things

## Go Crazy
it is hard to get too much juiciness, so just add a ton of it
___
