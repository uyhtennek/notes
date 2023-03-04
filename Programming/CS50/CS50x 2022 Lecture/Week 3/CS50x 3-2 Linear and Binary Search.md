Let say we have 7 closed lockers (= array). Each contain a number.
`{ 4, 8, 6, 2, 7, 5, 0 }`.
but of course the are unknown to us because the lockers are closed.
`{ x, x, x, x, x, x, x}`
this is simulating how a computer look for an item in an array.

Now how do we find the number `0`?
___

# Linear Search
look for each looker one by one, left-to-right or right-to-left

since we don't know anything about the lockers and the numbers, we don't know if they are following some rules or patterns, Linear search is about as good as we can do.

Pseudo code:
```
For each door, from left to right
	If number 0 is behind door
		Return true
Return false
```

Running time:
$O(n)$, at most it takes $n$ times
$\Omega(1)$, at least it takes 1 time only[^1]
[^1]: by 1 time it means it loops 1 time only, but not it needs 1 step only

$\Theta$ ? no it doesn't apply here.

how about the inner steps for things like checking what's the number or returning true? don't we need to consider them?
in terms of efficiency, it doesn't matter. the `if` check and `return true` are needed anyway, how long or how much steps this 2 commands will take are not going to matter.
what will matter though, is how many times the 2 commands are going to fire. and that is $n$ times and that is the efficiency of the loop.
___

now we sorted the number behind the closed doors, it's now from small to big.
`{ x, x, x, 5, 6, 7, x}`

Now instead of finding `0`, how do we find number `6`?
___

# Binary Search
look to the middle of all items, then decide to go to the upper half or lower half, then jump to the middle again and repeat.

since we know the numbers are sorted from small to big, `6` is probably not at the start nor the end, so we start from the middle.
resulting we just need at most 3 steps to find the number `6` in stead of 7 steps when we are applying [[CS50x 3-2 Linear and Binary Search#Linear Search|Linear search]]

Pseudo code:
```
If no doors  <-- or Start point is greater than End point
	Return false
If number behind middle door
	Return true
Else if number < middle door
	Search left half
Else if number > middle door
	Search right half
```
notice the word **Search** implies that the program has a recursive structure

Running time:
$O(\log{}n)$, anytime an algorithm that's *dividing and conquering*, it's probably *logarithms*
	$\log{}n$ essentially refers to the maximum number of times we can divide $n$ by 2
$\Omega(1)$

notice that in best case, Binary search and Linear search both take only 1 times.
but who cares if it get lucky any way?
so generally we care about big O notation only,
only sometimes depending on the nature of the data given, we care about $\omega$
___

# Linear Search vs Binary Search
however, it's important to know that Binary search is made possible only when the data are **sorted**, isn't that take time too?
Sure. so it depends on our problem to decide whether we should sort the data first for better searching after, or just don't waste the time and search it anyway.

For example, if we have a Google-like problem, where we have so many so many users searching for many different web pages, we better incur the cost up front and sort the whole thing, then **every** request thereafter can use Binary search and just search faster.
Imagine using Linear search instead, how much more steps there gonna be?

In another case, what if we just need to analyze a big data one time only?
Even though the search is inefficient since we didn't spend time writing complex code to sort the data, but we can just go to sleep and 8 hours later the result is here any way.
But 8 hours later we wake up and discover the result is incorrect. Then we change some code and run it again, and we can go to sleep again.

so generally Linear search is just more convenient, until a point that it just wastes less time even after some time is costed for preparing the data to do Binary search

we can even spend time calculating when is this turning point, but usually it's not that much of a hard decision
___
