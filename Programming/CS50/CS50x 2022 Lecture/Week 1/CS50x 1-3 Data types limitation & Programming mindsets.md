# bits limit / overflow

try adding 2 numbers in C:
```C
int x;
int y;

1. x = 1; y = 1;
2. x = 100,000,000; y = 100,000,000;
3. x = 200,000,000; y = 200,000,000;

x + y = ?
1. 2
2. 200,000,000
3. -294,967,296
```

Why is that?
`int` usually uses 32 bits, which give us about **4 billion** numbers to use
but usually computers support negatvie number.
so usually `int` gives us **2 billion positive numbers & 2 billion negative numbers** to use.

so if we need to calculate `200,000,000 + 200,000,000`
we should use `long`

What happen if we do this?
```C
long x = 200,000,000;
long y = 200,000,000;

int z = x + y;
z = ?
```
`z` will ignore 32 bits of the result.

enought with *int*, how about *float*?
```C
float z = 1 / 10.0;

1. printf("%f\n", z);
2. printf("%.2f\n", z);
3. printf("%.50f\n", z);

// and then ther results are:
1. 0.100000
2. 0.10
3. 0.10000000149011611938476562500000000000000000000000
```

^605b9c

this is called *floating-point imprecision*.
___
# overflow explained

3 bits give us 7 numbers
```
000 -> 0
001 -> 1
010 -> 2
...
111 -> 7
```

what is the next number?
`1000 -> 8` if we have 1 more bit to use.
`000 -> 0` is what happen when we don't. because the first bit `1` doesn't have a place to store.

Fun fact: **Y2K problem**
on `1st January 2000`, many computers can't update their clock.

It turns out at that time a lot of computers didn't store year as 4 digits. Why?
Since centuries don't happen that often, and hardwares were expensive and memory was very tight early on, computers just stored the last 2 digits of any year.

So in those computer the next year of *1999* becomes *1900*.

Similarly the next *'world's end day'* in computer could be `19 January 2038, 03:14:07` (it's known as **Year 2038 problem**). How come?
Generally how computers nowadays keep track of time is they count the total number of seconds since the **epoch**, which is defined as `1st January, 1970`.
And computers usually use 32 bits to count the number of seconds since epoch, so it gives us `2147483647` seconds to count.

Then the next second after that, will make the first bit changed from 0 to 1.
Unfortunately that bit often represents negativity (but it doesn't make the next second -0 though, there is another fancier representation).
So the next second is `-2147483648`. Then it becomes `13 December 1901` for the computers.

So what is the solution?
Just more bits.
Computers now is using 64 bits instead of 32 bits to count the second since epoch. and it gives us many many many years to use.
___
# Truncation & type convertion
```C
float z;
1. z = 1 / 10.0
2. z = 1 / 10

// what are the results of z?
1. 0.100000
2. 0.000000
```
when dividing an *int* by an *int*, it gives us back an *int*
so the remainder of `1 / 10` just get thrown away, and it gives us the result *0*
Then when we store the result in a *float*, it just turns the *int* result *0* to a *float*. That's why the remainder *0.1* is lost.

The solution is obviously using the data type *float* instead of *int*. That's what we did in *1.*
But what if we can't do this because for some reason we only have access to *int* or *long*, or it just seems not properate to store *1* or *10* in *float*.

We actually can use type convertion:
```C
int x = 1;
int y = 10;

1. float z = x / (float) y;
2. float z = (float) x / y;
3. float z = (float) x / (float) y;

// all 3 are okay
// 3. is more explicit, but it suffices to just do 1. or 2.
```
___
# float representation

```C
int main(void)
{
	float amount = get_float("Dollar amount: ");
	int pennies = amount * 100;
	printf("Pennies: %i\n", pennies);
}

1. Dollar amount: .99
   Pennies: 99
   
2. Dollar amount: 1.23
   Pennies: 123

3. Dollar amount: 4.20
   Pennies: 419
```

Why *3.* is wrong?
Computer can't represent *4.20* accurately. Instead it is *4.199999...*
Then *4.199999... \* 100* gives us *419.9999...*
And finally changing it to an *int*, gives us *419*.

So the solution is rounding the result: `int pennies = round(amount * 100);`
(`round()` function comes from the `<math.h>` library)
___
# hidden mistakes lead big accidents
Fact: **Boeing airplane can't run for 248 days**

The Boeing airplane software was using a 32-bit integer counting up tenths of a second to keep track of something related to it's electrical power.

After *248 days* of the airplane software being continuously on, which is quite common in the airplane industry to make every dollar count keeping the planes up and running all the time, the 32-bit number would overflow and the power would shut off on the airplane, as a side effect because of some undefined behavior.

The temporary solution by Boeing at the time was, essentially:
> Well, have you rebooted your plane?

Until they roll out an actual software patch.
___
# stupid question in if

A
```C
if (x < y)
{ }
else if (x > y)
{ }
else if (x == y)
{ }
```

B
```C
if (x < y)
{ }
else if (x > y)
{ }
else
{ }
```

A is not a well-designed code. Why?
It's doing the same thing as B, but it asks one more question which is `else if (x == y)`.
That's more code to run and that's more operation time the computer need.
So **skip unnecessary logics**.
___
# avoid hard-code

A
```C
if (points < 2)
{ }
else if (points > 2)
{ }
else
{ }
```

B
```C
int mine = 2;
if (points < mine)
{ }
else if (points > mine)
{ }
else
{ }
```

B is better designed.
As the code expand, we don't want to hard code the number `2` every where. It is difficult to keep track of.
So, instead, we should have a variable that helps us keep track of all the places that this number is used.

But it is possible that we somehow changed the value of `mine`. Maybe it's by accident. Maybe we forget what it does.
By giving a variable a prefix `const`, it makes this variable not changable. It becomes a constant.

`const int MINE = 2;`
It is a convention to give a constant variable name with all upper-case.
as a visual reminder that telling this variable is a constant.
___
# avoid repedition using loop

```C
printf("meow\n");
printf("meow\n");
printf("meow\n");
```
Why this is not well-designed?
1. inconsistent mistake can be made
2. have to update all the lines if we want to change something
3. not lazy enough

```C
while (true)
{ }
```
this will run forever

how about we just want to print 3 times?
```C
int i = 0;
while (i < 3)
{
	printf("meow\n");
	i++;
}
// same as
int i = 1;
while (i <= 3)
{
	printf("meow\n");
	i++;
}
// same as
int i = 3;
while (i > 0)
{
	printf("meow\n");
	i--;
}
```
using `i`, then `j`, then `k` is the convention when ever we need to count something
using any of the 3 options is totally fine. however the convention is the first one:
* starts at 0
* counts up, unless in some cases we do want to count down

a `for loop` can do the same:
```C
for (int i = 0; i < 3; i++)
{
	printf("meow\n");
}
```

Which is better?
> Eh. It's the same.

Except there is a difference in scope:
* For the `while loop`, `i` exists **outside** the block
* For the `for loop` though, `i` exists **inside** the block
___
# prompting input with do-while loop

assuming we need to prompt a number from the user, and that number is only valid if it's larger than or equal to 0.

we may want to use a *do-while loop*.
because a *do-while loop* run the loop one time first anyway before checking the condition.
```C
int main(void)
{
	int n;
	do
	{
		n = get_int("Width: ");
	}
	while(n < 1);
	
	// some code
	// which need n to be >= 0
}
```

What it does is when ever the user entered a number smaller than 1, we prompt him again, until we can get a valid number from the user, in this case it's when n is larger than or equal to 0.

Why can't we use *while loop* or *for loop*?
because they check condition first before running the loop.
In this case there is no condition to check before user input a value, since *n* doesn't have a value yet, so we need the *do-while loop*.
___
# break

we use the `break` keyword to break out of a loop.
```C
// in the last example, we used a do-while loop:
do
{
	n = get_int("Size: ");
}
while(n < 1);

// it's the same as:
while(true)
{
	n = get_int("Size: ");
	if (n > 0)
		break;
}
```
same for any other loop for instance *for loop*.
___
