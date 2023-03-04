[Cartesian Coordinate Systems - 3D Math Primer for Graphics and Game Development](https://gamemath.com/book/cartesianspace.html)
___

# Introduction
3D math is all about measuring locations, distances, and angles

The most frequently used framework to perform 3D space calculations using a computer is called the *Cartesian coordinate system*
* invented by a French philosopher, physicist, physiologist, and mathematician, *René Descrartes*
	* 勒內 笛卡兒
	* lived from 1596 to 1650
* well-known for answering the question "How do I know something is true?"
	* Ancient Greeks proposed
		* *ethos*, which bascially means "because I told you so"
		* *pathos*, "because it would be nice"
		* *logos*, "because it makes sense"
	* he set about figuring it out for himeself with a pencil and paper
___

# 1D Mathematics


## Discrete mathematics

*natural numbers*, or *counting numbers*, were invented millennia ago
* "one sheep", "two sheep", "three sheep", ....
* then at some point it is too much work so people go "many sheep"
* at some point people immortalize the concept of zero ("no sheep")

eventually people figured out various systems to name all of the natural numbers using digits
thus, mathematics was born

At some point in history, maybe some fast alkers sold sheep that they didn't actually own
thus simultaneously inventing the important concepts of debt and negative numbers
* the fast talker would in fact own "negative one" sheep
* leading to the discovery of the *integers*
	* which consist of the naturual numbers and their negative counterparts

The concept of poverty probably predated that of debt, some people could affort to purchase only half a dead sheep, or perhaps only a quarter
thus people would use fractional numbers
* which consist of one integer divided by another
	* eg. $2/3$, $111/27$
* mathematicians called them *rational numbers*
* at some point, people became lazy and invented decimal notation
	* so we would write "3.1415" instead of the tedious $31415/10000$


## Continuous mathematics

After a while people noticed that some numbers were not expressible as rational numbers
* classic example is the ratio of the circumference of a circle to its diameter, $\pi$
	* if expressed in decimal notation, it requires an infinite number of decimal places
* also the so-called *real numbers*
	* which include the rational numbers and numbers such as $\pi$
* it forms the basis of most forms of engineering
	* creating much of modern civilization
* the cool thing about real numbers is that, they are uncountable
	* the study of natural numbers and integers is called *discrete mathematics*
	* the study of real numbers is called *continuous mathematics*
		* a real number is a number between two ration numbers or natural numbers

The truth is, however, real numbers are nothing more than a polite fiction 客套話 / 虛言假語
* they are a relatively harmless delusion, as any reputable physicist will tell you
	* real numbers are not real at all
* The universe seems to be not only discrete, but also finite
	* there are a finite amount of discrete things in the universe
	* we can only count to a certain fixed number, and thereafter run out of things to count on
* so we can describe the universe using only discrete mathematics
	* and only requiring the use of a finite subset of the natural numbers at that
* there may be an alien civilization with a level of technology exceeding ours who have never heard of continuous mathematics
	* so is the fundamental theorem of calculus, or the concept of infinity, or $\pi$
	* and build starships, using 3.14159 or 3.1415926535897932384626433832795...

Then why do we use continuous mathematics? Because it is useful for engineering.
* despite the real world is finite and discrete, so is the computer
* C++ gives us a variety of different forms of number
	* `short`, a 16-bit integer, storing 65,536 different values
		* it sounds like a lot but it isn't adequate for measuring distances inside any reasonable kind of virtual reality that take people more than a few minutes to explore
	* `int`, a 32-bit integer, storing 4,294,967,296 different values
		* which is probably enough for most purposes
	* `float`, a 32-bit value, that can store a subset of the rationals
		* slightly fewer than 4,294,967,296
		* since a bit is used for indicating the floating point
	* `double`, similar to `float` except it used 64 bits instead of 32

some misguided people may think it's a matter of choosing between discrete `short` and `int` versus continuous `float` and `double`
* but it is more a matter of precision
	* they are all discrete after all
* older books on computer graphics may advise using integers because floating-point hardware is slower than integer hardware
	* which is no longer the case
	* in fact, nowadays we have dedicated floating point vector procesors that make floating-point arithmetic faster than integer in many common cases
* so which should we use?

> [!tip] The First Law of Computer Graphics
> If it *looks* right, it *is* right.

___

# 2D Cartesian Space
*Cartesian* is mostly just a fancy word for *rectangular*

it is applied every where in our daily life
* eg. floor plans of a house, street map, football pitch, chess, etc.


## The Hypothetical City of Cartesia
Let's imagine a fictional city named Cartesia.

![map_of_cartesia.png (656×656) (gamemath.com)|492](https://gamemath.com/book/figs/cartesianspace/map_of_cartesia.png)

* all streets run east-west and north-south
* in the middle are Center Street and Division Street
* the naming convention used may not be creative, but certainly is practical
	* we can easily find the donut shop at North 4th and West 2nd without a map
	* it's also easy to determine how far away 2 addresses is


## Arbitrary 2D Coordinate Spaces
before Cartesia was built, there was nothing but a large flat area of land

the city planners need to arbitrarily decide
* where the center of town would be
* which direction to make the roads run
* how far apart to space the roads
* and so forth

much like the Cartesia city planners laid down the city streets,
we can establish a 2D Cartesian coordinate system anywhere we want
* eg. a piece of paper, a chessboard, a chalkboard, a slab of concrete, or a football field

![2d_cartesian_space.png (360×360) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/2d_cartesian_space.png)

A 2D Cartesian coordinate space is defined by two pieces of information
* the origin, the "center" of the coordinate system
* 2 straight lines that pass through the origin
	* each line is known as an axis, and extends infinitely in two opposite directions
	* they are perpendicular to each other
		* actually, they don't have to be
		* but it is convenient to look at it that way

There are a few significant differences between Cartesia and an abstract mathematical 2D space
* Cartesia has official city limits
	* land outside of the city limits is not considered part of Cartesia
	* a 2D coordinate space, however, extends infinitely
* In Cartesia, the roads have thickness. But lines in an abstract coordinate don't.
* In Cartesia, we can drive only on the roads. In an abstract coordinate space, we use points.
	* not just the "roads"
	* the grid lines are drawn only for reference

The horizontal axis is called the *x*-axis, with positive *x* pointing to the right
The vertical axis is the *y*-axis, with positive *y* pointing up
* this is the customary (習慣性) orientation for the axes in a diagram
* note that "horizontal" and "vertical" are terms that are inappropriate in many practice
	* eg. the coordinate space on top of a desk
		* both axes are "horizontal", neither axis is really "vertical"

The city planners of Cartesia could have made Center Street run north-south instead of east-west.
Or they could have oriented it at a completely arbitrary angle.
* eg. Long Island, New York, is reminiscent of Cartesia, where for convenience the "streets" run across the island and the "avenues" run along its long axis

* In the same way, we're free to orient our axes in any way that is convenient to us
* also we msut decide for each axis which direction we consider to be positive
	* eg. images on a computer screen, use a coordinate system with the origin in the upper left-hand corner, +*x* points to the right and +*y* points down, rather than up

![screen_space.png (300×300) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/screen_space.png)

![2d_axes_orientations.png (581×281) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/2d_axes_orientations.png)

All 2D coordinate systems are essentially, "equal"
* the first rectangle is the normal orientation
	* +*x* points to our right, and +*y* points to up
* the top row of rectangles can be rotated to the same orientation
* the second row of rectangles can be reversed and then rotated to the normal orientation
* this is not the case in 3D though


## Specifying Locations in 2D
using Cartesian Coordinates

A coordinate space is a framework for specifying location precisely.
* we specify a location with two coordinates
	* one in the horizontal dimension, East 2nd Street
	* one int the vertical dimension, North 4th Street
* "Meet you at $(2,4)$."
	* $(2,4)$ is an example of what are called *Cartesian coordinates*

* In 2D, two numbers are used to specify a location
	* so it's called *two*-dimensional space
* In 3D, we use three numbers

* 2 in $(2,4)$, is called the *x*-coordinate
* 4 in $(2,4)$, is called the *y*-coordinate

Each of the two coordinates specifies which side of the origin the point is on, and how far away the point is from the origin in that direction
* each coordinate is the *signed distance* to one of the axes
	* ie. positive in one direction and negative in the other
	* essentially we use positive coordinates for east and north streets and negative coordinates for south and west streets
* measured along a line parallel to one axe and from the another axe

![2d_locating_points.png (294×282) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/2d_locating_points.png)

* *x*-coordinate designates the signed distance from the point to the *y*-axis, measured along a line parallel to the *x*-axis
* *y*-coordinate designates the signed distance from the point to the *x*-axis, measured along a line parallel to the *y*-axis

![2d_labeled_points.png (450×450) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/2d_labeled_points.png)

Here are several points and their Cartesian coordinates
* notice that points to the left of the *y*-axis have negative *x* values, and those to the right of the *y*-axis have positive *x* values
* likewise, points with positive *y* are located above the *x*-axis, and points with negative *y* values are below the *x*-axis
* also notice that *any* point can be specficied, not just the points at grid line intersections

* also notice that a vertical grid line is composed of points that all have the same *x*-coordinate
	* a vertical grid line, and *any* vertical line, marks a line of constant *x*
* likewise, a horizontal grid line marks a line of constant *y*
* we'll come back to this idea when we discuss polar coordinate spaces
___

# 3D Cartesian Space

It might seen at first that 3D space is only "50% more complicated" than 2D
* after all, it's just *one* more dimension
* but this is not the case

3D space is *more* than incrementally more difficult than 2D space for humans to visualize
* illustrations in books and on computer screens are 2D, after all
* it's frequently the case that a problem that is "easy" to solve in 2D is much more difficult or even undefined in 3D

Still, many concepts in 2D do extend directly into 3D
* we frequently use 2D to establish an understanding of a problem and develop a solution
* then extend that solution into 3D


## z-axis

In 3D, we require three axes to establish a coordinate system
* the first two axes are called the *x*-axis and *y*-axis
	* it's not accurate to say that these are the *same* as the 2D axes; more on this later
* the third axis is the *z*-axis
* usually, we set things up so that all axes are mutually perpendicular

![3d_cartesian_space.png (347×324) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/3d_cartesian_space.png)

In 2D, it is customary for *+x* to point to the right and *+y* to point up
* sometimes *+y* may point down though
* but in either case, *x*-axis is horizontal and *y*-axis is vertical

In 3D, however, it is not very standardized
* different authors and fields of study have different conventions

In 3D, any pair of axes defines a plane
* it contains the two axes and is perpendicular to the third axis
* *xy* plane is perpendicular to the *z*-axis; *xz* plane is perpendicular to the y-axis; *yz* plane is perpendicular to the *x*-axis.
* we can consider any of these planes a 2D Cartesian coordinate space in its own right
	* eg. in the above image, the 2D coordinate space of the "ground" is the *xz* plane


## Specifying Locations in 3D

In 3D, points are specified using three numbers, *x*, *y*, and *z*
* they give the signed distance to the *yz*, *xz*, and *xy* planes, respectively
* the distance is measured along a line parallel to the axis
* It is just a straightforward extension of the process for 2D

![3d_locating_points.png (285×338) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/3d_locating_points.png)


## Left-handed versus Right-handed Coordinate Space

As we dicussed in [[#Arbitrary 2D Coordinate Spaces]], all 2D coordinate systems are "equal" (assuming perpendicular axes)
* we can always find a way to set things back into the standard orientation

How this idea extends into 3D?
* we started with *+z* points into the page
* how about flipping it, so *+z* points out of the page? it is totally fine that we do so.

* Now, can we rotate the coordinate system around such that things line up with the original coordinate system?
* As it turns out, we cannot.
	* we can line up two axes at a time, but the third axes always points in the wrong direction

All 3D coordinate spaces are not equal
* there are exactly two distinct types of 3D coordinate spaces
	* *left-handed* coordinate spaces, and
	* *right-handed* coordinate spaces
* If two coordinate spaces have the same handedness, they can be rotated such that the axes are aligned. If they are of opposite handedness, then it is not possible.

What do "left-handed" and "right-handed" mean?
<span>
<img src="https://gamemath.com/book/figs/cartesianspace/left_handed_coordinate_space.png">
<img src="https://gamemath.com/book/figs/cartesianspace/right_handed_coordinate_space.png">
</span>

* the only difference is that *+x* points to right or left

actually, there is one more difference, which is the "positive rotation"
* let's say we have a line in space and we need to rotate about this line
	* we call this line an *axis of rotation*
	* the word *axis* here, is not related to the cardinal axes, the *x*-, *y*-, or *z*-axis, at all
* an axis of rotation can be arbitrarily oriented

* now if we want to "rotate $30\degree$ about the axis", how do we know which way to rotate?
	* we need to agree that one direction of rotation is the positive direction, and the other direction is the negative direction
* the standard way to tell that in a left-handed coordinate system is called the *left-hand rule*
	* first, we must define which way our axis "points"
		* just like the normal cardinal axes, the axis of rotation is infinite in length, and have a positive and negative end

> [!info] The left-hand rule
> With your thumb pointing towards the positive end of the axis of rotation. Positive rotation about the axis of rotation is in the direction that your fingers are curled.
> 
> The is a right-hand rule and it is the same thing.

<span>
<img src="https://gamemath.com/book/figs/cartesianspace/left_hand_rule_rotation.png">
<img src="https://gamemath.com/book/figs/cartesianspace/right_hand_rule_rotation.png">
</span>

In a left-handed coordinate system, positive rotation rotates *clockwise*, when viewed from the positive end of the axis.
In a right-handed coordinate system, positive rotation is *counterclockwise*.

Any left-handed coordinate system can be transformed into a right-handed coordinate system
* or vice versa
* by swapping the positive and negative ends of one axis
	* notice that if we flip *two* axes though, it is the same as rotating the coordinate space $180\degree$ about the third axis
		* which does not change the handedness of the coordinate space
* another way to toggle the handedness is to exchange two axes

Both left-handed and right-handed coordinate systems are perfectly valid
* neither is "better" than the other
* eg. some newer computer graphics literature uses left-handed coordinate systems, whereas traditional graphics texts and more math-oriented linear algebra people tend to prefer right-handed coordinate systems
	* these are gross generalizations though, so always check to see what coordinate system is being used
* in many cases it's just a matter of a negative sign in the *z*-coordinate
	* so if something doesn't look right, try flipping the sign on the *z*-axis


## Conventions Used in This Book

In 2D, we have 8 ways to assign the axes
In 3D, we have 48 different combinations
* 24 of these are left-handed, and 24 are right-handed

certain tasks can be easier if we adopt the right conventions
* however, it's not a big deal if we don't use the right convention
* it is better to stick with one convention than use the right convention

In fact, the choise is most likely thrust upon us by the engine or framework we are using
* because very few people start from scratch these days

All of the basic principle discussed are applicable regardless of the conventions used
* most of the equations and techniques given are applicable regardless of convention
* however, in some cases there are some slight, but critical, differences in application dealing with left-handed versus right-handed coordinate spaces

In this book, we use a left-handed coordinate system
* *+x*, *+y*, and *+z* directions point right, up, and forward, respectively
* in some situations where "right" and "forward" are not appropriate, *+x* is "east" and *+z* is north
	* eg. world coordinate space

![3d_conventions.png (379×358) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/3d_conventions.png)
___
