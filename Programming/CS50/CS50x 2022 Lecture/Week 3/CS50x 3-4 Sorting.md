 let see our unsorted array
`{ 5, 2, 7, 4, 1, 6, 3, 0 }`
how can we sort it?
___

# Selection Sort
select the smallest element and swap it with the 1st element
```
step 1
{ 5, 2, 7, 4, 1, 6, 3, 0 }
	5 -> 2 -> 7 -> 4 -> 1 -> 6 -> 3 -> 0
	0 is the smallest, swap 2 and 0

step 2
{ 0, 2, 7, 4, 1, 6, 3, 5 }
	2 -> 7 -> 4 -> 1 -> 6 -> 3 -> 5  // note that the computer doesn't stop at 1
	1 is the smallest, swap 2 and 1

step 3, start from 3rd
{ 0, 1, 7, 4, 2, 6, 3, 5 }
	7 -> 4 -> 2 -> 6 -> 3 -> 5
	2 is the smallest, swap 7 and 2

step 4, start from 4th
{ 0, 1, 2, 4, 7, 6, 3, 5 }
	4 -> 7 -> 6 -> 3 -> 5
	3 is the smallest, swap 4 and 3

...
```
in addition, in step 2, the computer doesn't keep looking at the rest of the numbers for the smallest number after it reaches `1`. why is that?
it's because in this case we assume the computer can only remember 1 variable.
so although the computer did loop through the array once in step 1, in step 2 it just forgets the whole array again and it doesn't sure if there is any number smaller than `1` when it reaches `1`.

what happen if we give it a second variable?
1. in step 1, the computer remember the 2 smallest number, `0` and `1`.
    then it figures out that "okay, `0` is the smallest." and then swap it with the 1st element.
    and `1` is the second smallest so swap it with the 2nd element
2. in step 2, the computer forgets everything again, but this time it starts from 3rd element
so it is faster to sort, but this approach sacrifice additional memory space for sure

now back to using a single variable, for simplicity

Pesudo code
```
For i from 0 to n-1
	Find smallest number from numbers[i] to numbers[i-1]
	Swap smallest number with numbers[i]
```

Efficiency:
1. $n + (n - 1) + (n - 2) + ... + 1$
2. $=\frac{n(n + 1)}{2}$
3. $=\frac{n^2 + n}{2}$
4. $=\frac{n^2}{2} + \frac{n}{2}$
and then if $n$ is really really big, only $n^2$ is the dominating factor here
so it's $O(n^2)$

but how is the best case scenario, which is the coming data is already sorted?
the computer still need to run the loop this many time to check if the numbers are sorted
so it's still $\Omega(n^2)$, thus it's $\theta(n^2)$
___

# Bubble Sort
in each loop compair an adjacent pair of items first.
the last items are the first to be sorted = bubbling up.
```
step 1
{ 5, 2, 7, 4, 1, 6, 3, 0 }
	5 and 2 are in wrong order, swap them
	5 and 7 are in right order, do nothing
	7 and 4 are in wrong order, swap them
	7 and 1 are in wrong order, swap them
	7 and 6 are in wrong order, swap them
	7 and 3 are in wrong order, swap them
	7 and 0 are in wrong order, swap them

step 2
{ 2, 5, 4, 1, 6, 3, 0, 7 }
	2 and 5 are in right order
	5 and 4 are in wrong order
	5 and 1 are in wrong order
	5 and 6 are in right order
	6 and 3 are in wrong order
	6 and 0 are in wrong order
	// 7 is skipped since the last item is already sorted

step 3
{ 2, 4, 1, 5, 3, 0, 6, 7 }
	2 and 4
	4 and 1
	4 and 5
	5 and 3
	5 and 0
	// last 2 items are skipped

step 4
{ 2, 1, 4, 3, 0, 5, 6, 7 }
	// last 3 items are skipped

step 5
{ 1, 2, 3, 0, 4, 5, 6, 7 }
	// last 4 items are skipped

step 6
{ 0, 1, 2, 3, 4, 5, 6, 7 }
	// last 5 items are skipped

step 8 = final step
{ 0, 1, 2, 3, 4, 5, 6, 7 }
	// last 6 items are skipped
we looped through the array again. we recognized we didn't do anything this time, which mean the array is already sorted. so we call it finished.
```

Pesudo code
```
Repeat n-1 times
	For i from 0 to n-2-i
		// n-2 so that numbers[i+1] will not go out of boundary
		// n-2-i because we don't need to sort the last items, which is sorted
		If numbers[i] and numbers[i+1] out of order
			Swap them
	If no swaps
		// if didn't add this, the loop will run n-1 times anyway
		// even though the numbers are all already sorted
		Quit
```

Efficiency:
1. $(n-1)\times(n-1)$
2. $n^2 - 1n - 1n + 1$
3. $n^2 - 2n + 1$
then $n^2$ is the dominating factor
so it's $O(n^2)$, but it's not exactly like [[CS50x 3-4 Sorting#Selection Sort|Selection Sort]]

how about the lower bound?
all items are sorted already
= the 1st loop didn't have any swap
= $\Omega(n)$, or more precisely $\Omega(n-1)$
and sometimes it's an advantage to not comparing things precisely. and the details don't matter when $n$ is really large anyway.
___

# Selection sort vs Bubble sort

Selection sort:
* $O(n^2)$
* $\Omega(n^2)$

Bubble sort:
* $O(n^2)$
* $\Omega(n)$

so Bubble sort does sound better. should we just always use Bubble sort?
well, yes, if we believe we can benefit over time from a lot of good case scenarios
but actually we have options that are better than both of these.
Way better and fundamentally better just like how [[CS50x 3-2 Linear and Binary Search#Binary Search|Binary search]] is way better than [[CS50x 3-2 Linear and Binary Search#Linear Search|Linear search]].
___

# Recursion
it's a programming technique (actually mathematical technique) whereby a function calls itself.

let say we want a program to print out a half pyramid like this:
```
#
##
###
####
```

```c
void draw(int n)
{
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < i + 1; j++)
		{
			printf("#");
		}
		printf("\n");
	}
}
```
this surely will is correct. but we can see there is a recursive structure[^1] in this task.
[^1]: recursive structure: a structure that's defined in terms of itself

what does it mean?
think about what does the 4th row of the pyramid looks like?
`####`, right?
but we can also think of it as the *3rd row of pyramid plus 1 has*: `###` + `#`
okay then what's a pyramid of height 3? it's *a pyramid of height 2 plus 1 row*
what's a pyramid of height 2? it's a *pyramid of height 1 plus 1 row*
what's a pyramid of height 1? it's a *pyramid of height 0 plus 1 row*
and then we can just stop at pyramid of height 0 since otherwise it'll go negative

now leverages this idea to our code
```c
void draw(int n)
{
	if (n <= 0) // escape point
	{
		return;
	}
	
	// draw the upper row first before the current row
	draw(n - 1);
	
	// draw the row
	for (int i = 0; i < n; i++)
	{
		printf("#");
	}
	printf("\n");
}
```

what happen if we don't have an escape for this recursive function?
the compiler is smart enough to give us an error:
`error: all paths through this function will call itself`

instead if we used `if (n == 0)`, the compiler recognize there is an escape so it'll not give the error.
but then if we input negative number as `n`, the program entered an infinite loop and eventually it will get stopped:
`Segmentation fault (core dumped)`[^2]
[^2]: this message means the program have somehow touched memory that it shouldn't have

see more in [[CS50x 3-5 More in Recursion|here]].
___

# Merge Sort
Sort smaller arrays and then merge those arrays together in sorted order.
think of it as instead of sorting a 6-elements array, we sort 6 1-element arrays.

Pseudo code
```
If only one number
	Quit
Else
	Sort left half of numbers
	Sort right half of number
	Merge sorted halves
```
the tricky part here is the **Merge** word. what does it mean?

let say we already sorted the left half and right half. all remaining is just to merge the two.
```
2457 0136

we just need to compare the left most number on each half. 2 and 0
who is smaller? 0
so we put it into the first place of a new array
0___ ____

and then we do the compare again:
2457 136
1 is smaller. so we do 01__ ____.

and then compare the next:
2457 36
...
```

now look back to the **Sort** part.
it did better than both Selection sort and Bubble sort. but how?

notice it's using not any of these 2 methods, but recursion.
let follow the pseudo code and see how it's executed.
```
52741630

step 1 - sort left half of 52741630
5274

step 2 - sort left half of 5274, since it's recursion
52

step 3 - sort left half of 52
5
we quit at 5 because it's only 1 number

step 4 - sort right half of 52
2
we quit at 2 because it's only 1 number

step 5 - 5 and 2 are sorted, merge them
5 2 -> 2
5   -> 25

step 6 - sort right half of 5274
74

step 7 - sort left half of 74
7
quit

step 8 - sort right half of 74
4
quit

step 9 - 7 and 4 are sorted, merge them
7 4 -> 4
7   -> 47

step 10 - 25 and 47 are sorted, merge them.
25 47 -> 2___
 5 47 -> 24__
 5  7 -> 245_
    7 -> 2457

left half of the original is already sorted. now sort the right half.

step 11 - sort right half of 52741630
1630

...

sometimes later both left half and right half of the original are sorted
2457 0136

final step - merge them
2457 0136 -> 0_______
2457  136 -> 01______
2457   36 -> 012_____
 457   36 -> 0123____
 457    6 -> 01234___
  57    6 -> 012345__
   7    6 -> 0123456_
   7      -> 01234567
```

look at one more example
```
521364

step 1
521
// how to go on from here?
// we just need to decide either the left half or right half has only 1 element
// and the other half has 2 elements
// but better to be consistent that the same half is the smaller
// throughout the whole sorting process

step 2
5
quit

step 3
21

step 4
2
quit

step 5
1
quit

step 6 - 2 and 1
2 1 -> 1_
2   -> 12

step 7 - 5 and 12
5 12 -> 1__
5  2 -> 12_
5    -> 125

step 8
364

step 9
3
quit

step 10
64

step 11
6
quit

step 12
4
quit

step 13 - 6 and 4
6 4 -> 4_
6   -> 46

step 14 - 3 and 46
3 46 -> 3__
  46 -> 346
// did we skipped 34_?
// no. because everything on one side is placed
// so just place all of the remaining from the other side to the rest

final step - 125 and 346
125 346 -> 1_____
 25 346 -> 12____
  5 346 -> 123___
  5  46 -> 1234__
  5   6 -> 12345_
      6 -> 123456
```

wow it's so complicated. how come it's actually a faster algorithm?
1. unlike Selection Sort and Bubble Sort, we don't need to keep looping the array back and forth
2. everytime we do merge, each element is only moved once
3. how many times did we divide in half the list? it's actually $\log{}n$

but this approach involves more than just 1 array unlike Selection sort and Bubble sort, that is when ever we do merge.
in the above case the elements are moved 3 times before finally sorted.
but actually we just need 1 additional array of the same size $n$ to do the job since we can just keep jumping back and forth in the 2 arrays.

Efficiency:
$O(n\log{}n)$
$\Omega(n\log{}n)$
= $\theta(n\log{}n)$
so if we have a data source that is usually sorted, it maybe a good idea to stick with [[CS50x 3-4 Sorting#Bubble Sort|Bubble Sort]], since we can escape at where all items are sorted, but in the general case where the data is unsorted surely [[CS50x 3-4 Sorting#Merge Sort|Merge Sort]] is better.
___
