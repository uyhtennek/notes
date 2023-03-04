This part is a refresher to some mathematical knowledge
___

# Summation Notation

*Summation notation* is a shorthand way to write the sum of a list of things. It's sort of like a mathematical `for` loop.
$$\sum_{i=1}^6 a_i = a_1 + a_2 + a_3 + a_4 + a_5 + a_6$$

* the variable $i$ is known as the *index variable*
* the expressions above and below the summation symbol tell us how many times to execute our "loop" and what values to use for $i$ during each iteration

In this case, $i$ will count from 1 to 6. To execute the "loop",
1. we iterate the index through all the values specified by the control conditions
	* so iterate $i$ from 1 to 6
2. for each iteration, we evaluate the expression on the right-hand side of the summation notation
3. and add the value to the sum

Summation notation is also known as *sigma notation*
* because the cool-looking E shape symbol is the capital version of the Greek letter sigma
___

# Production Notation

Similar to summation notation, it takes the product of a serise of values, instead of sum
$$\prod_{i=1}^n a_i = a_1 \times a_2 \times {...} \times a_{n-1} \times a_n$$

the symbol $\prod$ is the capital version of the letter $\pi$
___

# Interval Notation

$[a,b]$ means "the portion of the number line from $a$ to $b$"
* or, "all numbers $x$ such that $a \leq x \leq b$"

* notice that this is a *closed* interval
	* meaning that the endpoints $a$ and $b$ are included in the interval
* an *open* interval is one in which the endpoints are excluded
	* it is denoted using parentheses instead of square brackets: $(a,b)$
	* it contains all $x$ such that $a \lt x \lt b$
* sometimes a closed interval is called *inclusive* and an open interval called *exclusive*

sometimes we could encounter *half-open* intervals
* which include one endpoint but exclude the other
* these are denoted with a lopsided notation
	* eg. $[a,b)$ or $(a,b]$

By convention, if an endpoint is infinite, we consider that end to be open
* eg. the set of all nonnegative number is $[0,\infty)$

Notice that the notation $(x,y)$ could refer to an opoen interval or a 2D point
Likewise, $[x,y]$ could be a closed interval or a 2D vector
___

# Angles, Degrees, and Radians

An angle measures an amount of rotation in the plane
* variables representing angles are often assigned the Greek letter $\theta$
* the most important units of measure used to specify angles are degrees $\degree$ and radians *rad*

![radians_measure_intercepted_arc.png (350×350) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/radians_measure_intercepted_arc.png)

Humans usually measure angles using *degrees*
* one degress measures 1/360th of a revolution
* $360\degree$ represents a complete revolution
	* 360 is a relatively arbitrary choice
	* it may have an origin in primitive calendars, like the Persian calendar, which divided the year into 360 days
		* it was never corrected 365 though perhaps because 360 is convenient
		* it has 22 divisors,and
		* a whole degree is precise enough in many circumstances

Mathematicians, however, prefer to measure angles in *radians*
* A radian measures arc length on a unit circle
	* a unit circle is a circle with radius of 1

The circumference of a unit circle is $2\pi$
* $\pi$ approximately equal to 3.14159265359
* therefore, $2\pi$ radians represents a complete revolution

$$\begin{align}
360\degree & = 2\pi\;rad\\
180\degree & = \pi\;rad
\end{align}$$
so,
$$\begin{align}
1\;rad &= (\frac{180}{\pi})\degree &\approx 57.29578\degree\\
1\degree &= (\frac{\pi}{180})\;rad &\approx 0.01745329\;rad
\end{align}$$
___

# Trig Functions

There are many ways to define the elementary trig functions.
We define them using the unit circle here.

![angle_in_standard_position.png (400×400) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/angle_in_standard_position.png)

The $(x,y)$ coordinates of the endpoint of the ray thus rotated, have special properties and are assigned special functions, known as the *cosine* and *sine* of the angle
$$\cos\theta = x,\;\sin\theta = y$$

* we can easily remember which is which in alphabetical order
	* $x$ comes before $y$, and $cos$ comes before $sin$

The *secant*, *cosecant*, *tangent*, and *cotangent* are also useful trig functions
* they can be defined in terms of the sine and cosine
$$\begin{align}
\sec\theta &= \frac{1}{\cos\theta},&\;\tan\theta &= \frac{\sin\theta}{\cos\theta},\\
\csc\theta &= \frac{1}{\sin\theta},&\;\cot\theta &= \frac{1}{\tan\theta} = \frac{\cos\theta}{\sin\theta}
\end{align}$$

We can form a right triangle using the rotated ray as the hypotenuse
* which is the side opposite the right angle

![hyp_adj_opp.png (330×330) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/hyp_adj_opp.png)
* *hyp*, *adj*, and *opp* refer to the lengths of the hypotenuse, adjacent leg, and opposite leg
* again, alphabetical order is a useful memory aid
	* "adjacent" and "opposite" are in the same order as the corresponding "cosine" and "sine"

Primary trig functions are defined by the following ratios:
$$\begin{align}
\cos\theta &= \frac{adj}{hyp}, &\;\sin\theta &= \frac{opp}{hyp}, &\;\tan\theta &= \frac{opp}{adj},\\
\sec\theta &= \frac{hyp}{adj}, &\;\csc\theta &= \frac{hyp}{opp}, &\;\cot\theta &= \frac{adj}{opp}
\end{align}$$

We can apply the equations even when the hypotenuse is not of unit length.
However, we can't apply them when $\theta$ is obtuse 銳角
* since we cannot form a right triangle with an obtuse interior angle

![angle_in_standard_position_arbitrary_length_ray.png (400×400) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/angle_in_standard_position_arbitrary_length_ray.png)

Still, we can calculate the ratio in exactly the same way
$$\begin{align}
\cos\theta &= \frac{x}{r}, & \sin\theta &= \frac{y}{r}, & \tan\theta &= \frac{y}{x}\\
\sec\theta &= \frac{r}{x}, & \csc\theta &= \frac{r}{y}, & \cot\theta &= \frac{x}{y}
\end{align}$$

Here are some common angles, expressed in degrees and radians, and the values of their principal trig functions
![[common-angles.png]]
___

# Trig Identities

A number of identities can be derived based on the symmetry of the unit circle:
$$\begin{align}
\sin(-\theta) &= -\sin\theta,& \cos(-\theta) &= \cos\theta,& \tan(-\theta) &= -\tan\theta\\
\sin(\frac{\pi}{2} - \theta) &= \cos\theta,& \cos(\frac{\pi}{2} - \theta) &= \sin\theta,& \tan(\frac{\pi}{2}-\theta) &= \cot\theta
\end{align}$$

Perhaps the most famous and basic identity concerning the right triangle,
is the *Pythagorean theorem* 畢氏定理
* it says the sum of the squares of the two legs is equal to the square of the hypotenuse
* $a^2 + b^2 = c^2$

![pythagorean_theorem.png (300×225) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/pythagorean_theorem.png)

By applying the Pythagorean theorem to the unit circle, we can deduce the identities
$$\sin^2\theta + \cos^2\theta = 1,\;1 + \tan^2\theta = \sec^2\theta,\;1 + \cot^2\theta = \csc^2\theta$$

We can take a trig function on the sum or difference of two angles
$$\begin{align}
\sin(a+b) &= \sin a \cos b + \cos a \sin b,\\
\sin(a-b) &= \sin a \cos b - \cos a \sin b,\\
\cos(a+b) &= \cos a \cos b - \sin a \sin b,\\
\cos(a-b) &= \cos a \cos b + \sin a \sin b,\\
\tan(a+b) &= \frac{\tan a + \tan b}{1 - \tan a \tan b},\\
\tan(a-b) &= \frac{\tan a - \tan b}{1 + \tan a \tan b}
\end{align}$$

If we apply the sum identities to the special case where $a$ and $b$ are the same, we get the following double angle identities
$$\begin{align}
\sin 2\theta &= 2\sin\theta\cos\theta,\\
\cos 2\theta &= \cos^2\theta - \sin^2\theta = 2\cos^2\theta - 1 = 1 - 2\sin^2\theta,\\
\tan 2\theta &= \frac{2\tan\theta}{1 - \tan^2\theta}
\end{align}$$

We often need to solve for an unknown side length or angle in a triangle, in terms of the known side lengths or angles
* for these problems, the *law of sines* and *law of cosines* are helpful
* these identities hold for any triangle, not just right triangles

![law_of_sines_law_of_cosines.png (345×173) (gamemath.com)](https://gamemath.com/book/figs/cartesianspace/law_of_sines_law_of_cosines.png)

Law of sines:
$$\frac{\sin A}{a} = \frac{\sin B}{b} = \frac{\sin C}{c}$$
Law of cosines:
$$\begin{align}
a^2 &= b^2 + c^2 - 2bc \cos A,\\
b^2 &= a^2 + c^2 - 2ac \cos B,\\
c^2 &= a^2 + b^2 - 2ab \cos C
\end{align}$$
___
