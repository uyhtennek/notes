# main() is not enough

there will be many branches and logics going on. so we want to introduce functions at some point to keep the logic seperated and clean.

so try writing everything in `main()` will eventually get ourselves some headache
___

# function
* also known as *procedures, methods, or subroutines*
* think of it as a black box
	* takes 0+ inputs
	* return 1 output
	* and we don't need to know how the black box works

**benefits**
* Organization
	* break up a complicated problem into more manageable subparts
* Simplification
	* it's easier to understand a 10 line function rather than 100 line or 1,000 lines
	* the smaller the easier to design, implement, and debug (and the better programming experience :D )
* Reusability
	* functions can be recycled
		* we've been recycling and using `printf()` for over 40 years, but it was only written 1 time
___

# using a function

1. firstly we need to declare it
	* write a *function declaration*, as known as [[CS50x 1-4 function in C#^dff102|prototype]]
	* always go atop our code, before we begin writing `main()`
	* a *function declaration* looks like `return-type name(argument-list);`
		* **return-type**: the kind of variable the function will output
		* **name**: name of the function
		* **argument-list**: inputs
			* each argument has a **type** and a **name**
			* sepearated by a comma ( **,** )
	* eg. `int add_two_ints(int a, int b);`
		* however if the input is important, remember to give it a symbolically meaningful name
		* rather than just `a` and `b`, but it's not so important so we just keep it simple
	* eg. `float mult_two_reals(float x, float y);`

2. define the function
* eg. using the *function declaration* `float mult_two_reals(float x, float y);`
```c
float mult_two_reals(float x, float y)
{
	float product = x * y;
	return product;
	// or just do
	return x * y;
}
```

3. call the function
* pass it some appropriate arguments
* assign its return value to something of the correct type
* eg. `int z = add_two_ints(x, y);`

miscellany:
* function can take no inputs
	* having a `void` argument list
	* eg. `main(void)`
* function can also don't have an output
	* having a `void` return type
___

# exercise: `valid_triangle()`

try writing a function called `valid_triangle`
* takes 3 real numbers representing the lengths of the 3 sides of a triangle as its arguments
* output either `true` or `false`
   depending on whether those 3 lengths are capable of making a triangle

rules about triagles:
* length must be positive
* sum of the lengths of any 2 sides must be greater than the length of the 3rd side

```c
bool valid_triangle(float a, float b, float c);

bool valid_triangle(float a, float b, float c)
{
	if (a <= 0 || b <= 0 || c <= 0)
	{
		return false;
	}
	if ((a + b <= c) || (a + c <= b) || (b + c <= a))
	{
		return false;
	}
	return true;
}
```
___

# call stack
what happen when we call a function?

1. the system sets aside space in memory (it's called **stack frame** or **function frame**) for that function to do its work
	* maybe we declared 3 int in a function, when we call this function, a room that has a size of 3 int is created for the function to use
2. when at some point in the function, another function get called.
	* it creates another **stack frame**
		* so it's possible that there are multiple *stack frame* at a time
		* like if `main()` called another function `move()`, then `move()` calls another function `direction()`
		* all 3 functions have open frames, but in general only 1 of those frames is active at a time
			* only the newly created frame is **pushed** onto the top of the stack
			* other 2 is just sitting there **on pause**, waiting to become the active frame again
	* those frames are arranged in [[C - 11 Dynamic memory allocation#stack and heap|stack]]
3. when a function finished executing, no matter if it's by `return` or it just get to the bottom line of the code, its frame is **popped off** of the *stack*
	* the frame below the *active frame* that just get *popped off* becomes the *active frame* again
	* the function picks up / resumes at immediately where it left off

this is actually why [[CS50x 3-4 Sorting#Recursion|recursion]] can works.
because the *function frames* that are not active, is just on pause rather than released.

eg.
```c
int fact(int n)
{
	if (n == 1)
		return 1;
	else
		return n * fact(n-1);
}

int main(void)
{
	printf("%i\n", fact(5));
}
```
1. `main()` is executed
2. `printf()` get called in `main()`, `main()` is paused
3. `fact(5)` get called in `printf()`, `printf()` is paused
4. `fact(4)` get called in `fact(5)`, `fact(5)` is paused
5. ....
6. ....
7. `fact(1)` get called in `fact(2)`
8. `fact(1)` returns `1`, didn't call any other function
	* `fact(1)` is done and got *popped back off* of the call stack
9. `fact(2)` get unpaused
	* it receives the value from `fact(1)`
	* eventually it's finished and `fact(2)` return another value
10. ....
11. ....
12. ....
13. `printf()` is unpaued, get back to `main()`
14. `main()` eventually finished
___
