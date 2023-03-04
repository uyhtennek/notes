# Computer's "eye"

let say we have an [[CS50x 2-3 Array#Array|array]] of int:
`4, 6, 8, 2, 7, 5, 0`
where is the number `0`?

we human can just see every number at once and tell it's at the last place
but computer cannot not.

when computer "see" this array, it's actually like this:
`X, X, X, X, X, X, X`
think of them as a closed door,
and only by opening them can let the computer see the number behind it.

so we use loops to loop through the items
usually it's left-to-right or right-to-left, or sometimes it's something more sophisticated
so that we can find something we're looking for
___

# Algorithms
search is the most omnipresent topics for obvious reason
and we do search by using some algorithms

how do we compare algorithms?
* **Correctness**, off course
	* what's the point if it doesn't give the right answers?
* **Design** = quality of code = quality of **performance**
	* if the data to search get bigger / more sophisticated / longer, performance will matter
	* easier to collaborate, and ultimately produce right code with higer probability

**Efficiency** of algorithms
* how fast or slow the algorithm is
* = how much *running times* = how much time does it take to run
* measured in seconds / milliseconds / **steps**
	* fewer steps = faster
___

# Big O Notation
it's what we used to describe the efficiency of an algorithm
> it means the running time of some algorithm is in the **order** of such and such, where such and such is some simple mathematical formula representation

remember the [[CS50x 0-2 Brief View of Programming|phone book case]]?
the 3 approaches to find a name in a phone book?
1. look at every page
2. look at every 2 page
3. *divide and conquer strategy*
    jump to the middle and discard the former or latter, and repeat

so let say the phone book has *n* pages, the steps the 3 approaches are gonna take are:
1. $n$
2. $n\div2$
3. $\log_{2}n$

notice in 1. and 2., the algorithms take on the order of *n* steps or *n/2* steps.
but if *n* get really really big, eventually it will get to a point that *n* and *n/2* don't really have much difference.
so computer scientists just consider they are the same. and represent both of them with *O(n)*. since in the big picture, they are functionally equivalent.

there are commonly seen running times in the real world, sorted by best to worst:
| Running Time | Name | Description |
| --- | --- | --- |
| $O(1)$ | Constant Time | take 1 step only no matter how big $n$ is |
| $O(\log{}n)$ | Logarithmic Time | the bigger $n$ is, the faster the algorithm |
| $O(n)$ | Linear Time | in worst case the algorithm will take $n$ steps |
| $O(n\log{}n)$ | Quasilinear Time | |
| $O(n^2)$ | Quadratice Time | |
| $O(n^3)$ | Cubic Time | |
| $O(n!)$ | Factorial Time | |
| ... | | |
why $O(1)$ is the best?
it means what ever size $n$ is, the algorithm still takes only a constant number of step

also there are other weird vocabulary:
$\Omega$ : lower bound on the running times of an algorithms   = minimum steps   = best case
$O$ : upper bound on the running times of an algorithms   = maximum steps  = worst case
$\Theta$ : case when lower bound and upper bound are the same
now we instead of saying *the lower bound / upper bound of an algorithm*
we say the $\Omega$ / $\omega$ / $\Theta$ or an algorithm...
___
