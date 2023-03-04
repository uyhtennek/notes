# declaring a function

```C
// declaring or creating a function
void meow(void)
{
	printf("meow\n");
}

// then how to call this function?
meow();
```

`meow.c` is now look like this:
```C
#include <stdio.h>

void meow(void)
{
	printf("meow\n");
}

int main(void)
{
	for (int i = 0; i < 3; i++)
	{
		meow();
	}
}
```
Notice the declaration of the `meow()` function is before `main()`

What if we declare `meow()` after `main()`:
```C
// meow.c
#include <stdio.h>

int main(void)
{
	for (int i = 0; i < 3; i++)
	{
		meow();
	}
}

void meow(void)
{
	printf("meow\n");
}
```
It will give an error
> implicit declaration of function 'meow' is invalid in C99[^1]

[^1]: C99 is the version of C from year 1999

C reads code **top to bottom**.
If we first try to use a function when it does not know what it is, it freaks out.

Which is a bit annoying since `main()` is our main function.
so it makes sense that we should see `main()` first before any other function if we read top to bottom.

Well, there is a solution to this.
We can create a prototype[^2] of a function without declaring its content.
[^2]: a fancy way to say 'declaring a function' ^dff102

```C
void meow(void);

int main(void)
{
	for (int i = 0; i < 3; i++)
	{
		meow();
	}
}

void meow(void)
{
	...
}
```
many other programming languages get rid of this need. allowing them to declare functions in any order.
but this is how they do so under the hood.

essentially this is how header file works too!
in `<cs50.h>`, there are the *get_string*, *get_int*, etc. functions. and then there are the prototypes.
So that when we *#include `<cs50.h>`*, the compiler knows what are the available functions in the header file. ^362abf

And it's why header files need to be declared on the top of the code.
___
# function with input

```C
// function with no input
void meow(void)
{ }

// function with an int as input
void meow(int n)
{ }

// remember to change the prototype of the function if we have
void meow(void); --> void meow(int n);
```

^1e399c

with no input we call a function by `meow()`. now with input we call this function by `meow(3)`, where the data type of the input need to match what we declared in the function, an *int*.

also functions can take more than 1 input.
```C
float discount(float price, int percentage)
{ return price * (100 - percentage) / 100; }
```
and remember to *change the prototype* if we have...

bare in mind the variable we declared as an input, they only live in the function.
that means we can't use that variable else where.
___
# function with return type

```C
float discount(float price);

float discount(float price)
{
	return price * .85;
}

// since the function return a value, we can store it.
float sale = discount(100.0);
```
___
