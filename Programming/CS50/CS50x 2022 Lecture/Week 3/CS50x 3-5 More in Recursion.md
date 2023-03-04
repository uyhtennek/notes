**example**: $n!$
> $n!$ equals all of the positive integers less than or equal to $n$, multiplied together
> eg. if $n$ is 5, $n! = 5 \times 4 \times 3 \times 2 \times 1 = 120$

if we have a function `fact(n)` that calculate factorial of $n$ for us:
```
fact(1) = 1
fact(2) = 2 * 1
fact(3) = 3 * 2 * 1
fact(4) = 4 * 3 * 2 * 1
...
```
thinking about it, isn't it equals to `fact(n) = n * fact(n-1)`?
```
fact(1) = 1
fact(2) = 2 * fact(1)
fact(3) = 3 * fact(2)
fact(4) = 4 * fact(3)
...
```
___

Every recursive function needs 2 parts:
* a **base case** (ie. escape point) to terminate the recursive process, in this case it's `fact(1)`
* a **recursive case**, which the recurrsion will actually occur

```c
int fact(int n)
{
	// base case
	if (n <= 1)
	{
		return 1;
	}
	
	// recursive case
	return n * fact(n - 1);
}
```
___

why recursion works?
```
let say we start from:
fact(5)
	it needs the result of fact(4)
		which needs the result of fact(3)
			which needs the result of fact(2)
				which needs the result of fact(1)
				and the result of fact(1) is 1

			then we have the result of fact(2)
		then we have the result of fact(3)
	then we have the result of fact(4)

finally we can get the result of fact(5)
```
___

in addition we can use a loop instead of recursion to do the same thing
```c
int fact(int n)
{
	int product = 1;
	while (n > 0)
	{
		product *= n;
		n--;
	}
	return product;
}
```
but they don't do it in the same way.
plus in some cases using recursion is easier than loop iteration.
___

it's possible to have more than one base case or recursive case

multiple base cases example: **Fibonacci number sequence**
* 1st element is 0
* 2nd element is 1
* $n^{th}$ element is the sum of $(n-1)^{th}$ and $(n-2)^{th}$ elements
* `0, 1, 2, 3, 5, 8, 13, 21, ...`

multiple recursive cases example: **The Collatz conjecture**
* if $n$ is 1, stop
* if $n$ is even, do $n/2$
* if $n$ is odd, do $3n+1$
* $n$ is any positive number
`collatz(n)` is how many steps it takes to get to 1
| n | `collatz(n)` | Steps |
| :-: | :-: | --- |
| 1 | 0 | 1 |
| 2 | 1 | 2 > 1 |
| 3 | 7 | 3 > 10 > 5 > 16 > 8 > 4 > 2 > 1 |
| 4 | 2 | 4 > 2 > 1 |
| 5 | 5 | 5 > 16 > 8 > 4 > 2 > 1 |
| ... | | |

```c
int collatz(int n)
{
	// base case
	if (n <= 1)
		return 0;
	
	// recursive case
	if (n % 2 == 0)
	{
		return 1 + collatz(n / 2);
	}
	else
	{
		return 1 + collatz(3 * n + 1);
	}
}
```
___
