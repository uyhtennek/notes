# Algorithm

say we want to find a name *John Harvard* in an address book, which sort all the names in alphabetical order. How?

1. Look through each page one by one
   eventually we will get there, just not very soon
2. Skip 1 page for every 2 pages
   yes we'll be faster, but it's possible that we missed the name
3. Start from the middle, see if we are ahead or behind the letter *J*, jump to the next middle, repeat

> Computer Science is not all about learning the computer world, but also how to harness intuition and ideas, and in the end express them using computer languages.
___
# Performance of Algorithm

Time to solve the problem using the 3 methods are:
1. $O(n)$
2. $O(n/2)$
3. $O(\log_{2}n)$
where *n* is the total number of names in the address book

*n* is usually representing *number* for programmers.
___
# Pseudocode

let write down the steps:
```pseudocode
1.  Open address book
2.  Open the middle page
3.  Look at page
4.  If John Harvard is on page
5.    Call him
6.  Else if the current page is ahead of letters *John Harvard*
7.    Open to middle of left half of the book
8.    Go back to line 3
9.  Else if the current page is behind letters *John Harvard*
10.   Open to middle of the right half of the book
11.   Go back to line 3
12. Else
13.   Quit
```
Pseudocode is just a *text-based* code to help programmers developing algorithms.

Now focus on the last 2 lines. Why are they there?
because the address book can have no *John Harvard* in it.

if we don't add this 2 lines, what will the program do?
We don't know. No one define it for the program.
___
# Repeatation is bad

* tideous
* easy to screw something up
* can't variate
___
# Abstruction

by abstructing away implementation details, we make a function easy to use and easy to read.
it'll very soon take benefit when we're working on something big.

> we make progress by solving sub-problems to a bigger problem.
___
# Not only Correctness, but Better Design
___
 