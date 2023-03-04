# Dynamic memory allocation

the 2 fundamental functions on this topic are:
* `malloc`
	* it means *memory allocate*
	* takes a number as input
		* which is how many bytes of memory that we need the operating system to find for us
	* returns the address of the first byte that the chunk of memory the compiler found
* `free`
	* as oppose, it frees a chunk of memory that we requested using `malloc`
		* then the operating system can use it for something else
		* remember it frees the memory that a pointer points to, not the pointer per se
	* if a program didn't free enough memory but keep asking for more, the computer will become very very slow, until at some point it just freezes
		* which is a common thing...
		* so try the best to always free every chunk of memory after we're done using it


continuing from the example of [[CS50x 4-2 string and address#need of strcpy|copying a string]]:
but before that, how many bytes we need if we want to copy a string `hi!`?
4 bytes. because each char, `h, i, !, \0`, takes 1 byte.

so how do we do copying a string?
```c
char *s = get_string("s: ");
char *t = malloc(strlen(s) + 1);  // require library <stdlib.h>

strcpy(t, s);

// make the first char to upper case
t[0] = toupper(t[0]);

printf("s: %s\n", s);
printf("t: %s\n", t);
```
> [! what happen?]
> 1. we prompt user for the value of `s`
> 2. `malloc()` finds memory in computre of size `strlen(s) + 1` for us to use
> 3. we created a pointer `t` and assign to it the address returned by `malloc()`
> 4. `strcpy()` copy the `h, i, !, \0` from `s` to `t`

however, there are a few improvements that we can make to this code.

1. firstly we didn't use `free()`
    it's a good practice to always free memory after we're done with it.
    so add it to the bottom of the code.
    also we should free `t` only but not `s`, because we didn't call `malloc` for `s`
    `get_string()` did, but it handled the `malloc` and `free` for `s` for us

2. it is possible that `malloc()` will fail, when it can't find enough available memory in computer maybe due to they're all occupied.

to prevent our program from doing something wrong due to this issue, we need to check if the `malloc` succeeded, by doing:
```c
char *s = get_string("s: ");
char *t = malloc(strlen(s) + 1);
// strlen(s) only count 'h', 'i' and '!', but not '\0'

if (t == NULL)
{
	return 1;
}
```
what is `NULL`? it's similar to `NUL`, which is a char value zero.
`NULL` is a zero for pointer
* it means a pointer is not valid
* when `malloc()` didn't find enough memory for us, it returns a special value `NULL`

3. `t[0] = toupper(t[0])` is a bit risky
    what if the user didn't type anything and just hitted enter when we prompt `t`
    in this case `t` doesn't even have the first letter. that is there is no `t[0]`.

we better check for it:
```c
if (strlen(t) > 0)
{
	t[0] = toupper(t[0]);
}
```

4. `return 0` at the end of the program to signify that the program executed to the end successfully
___

# Valgrind

now you can see there are a millions way to mess up memory
here is one example: `memory.c`
```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
	int *x = malloc(3 * sizeof(int));
	// sizeof() helps us find the size of a data type in this operating system
	x[1] = 72;
	x[2] = 73;
	x[3] = 33;
	
	// there are 2 mistakes here:
	1. index started from 1 instead of 0
	2. buffer overflow
	   it touched x[3], the forth item of x, but it only has 3 items
}
```
but if we do compile, it turns out to be successful.
if we do execute it too, it doesn't give any error and executed successfully until it reaches the end. it didn't encounter the [[CS50x 4-1 Memory#power and danger of pointers|segmentation fault]], since it is *nondeterminism*.

to debug this kind of memory, there is a tool, named **Valgrind**.
it's a memory debug tool that comes with many operating systems.
to use it, run `valgrind ./memory`.

its output using on our `memory.c` looks something like this:
```
==5902== Memcheck, a memory error detector
==5902== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==5902== Using Valgrind-3.15.0 and LibVEX; rerun with -h for copyright info
==5902== Command:./memory
==5902==
```
followed by:
```
==5902==
==5902== Invalid write of size 4
==5902==    at 0x401162: main (memory.c:9)
==5902==  Address 0x4bd604c is 0 bytes after a block of size 12 alloc'd
==5902==    at 0x483B7F3: mac (in /usr/lib/x86_64-linux-gnu/valgrind/...)
==5902==    by 0x401141: main (memory.c:6)
==5902==
...
```
this first part of this output, indicated the program can't write to 4th item of something, related to `memory.c` in line 9. and that is where we wrote `x[3] = 33;`

after we fix the problem, and run `valgrind ./memory`.
there is fewer message in the output:
```
...
==6435== HEAP SUMMARY:
==6435==     in use at exit: 12 bytes in 1 blocks
==6435==   total heap usage: 1 allocs, 0 frees, 12 bytes allocated
==6435==
==6435== 12 bytes in 1 blocks are definitely lost in loss record 1 of 1
==6435==    at 0x483B7F3: malloc (in /user/lib/x86_64-linux-gnu/valgrind/...)
==6435==    by 0x401141: main (memory.c:6)
==6435==
==6435== LEAK SUMMARY:
==6435==    definitely lost: 12 bytes in 1 blocks
==6435==    indirectly lost: 0 bytes in 0 blocks
...
```
notice that it said `12 bytes in 1 blocks are definitely lost in loss record 1 of 1`
it hints that there is a [[C - 11 Dynamic memory allocation#memory leak|memory leak]]
which means the blocks of memory are lost since we `malloc()` them but never `free()` them

after we fix it, run `valgrind ./memory` again.
the result should looks pretty good
```
...
==6812== HEAP SUMMARY:
==6812==     in use at exit: 0 bytes in 0 blocks
==6812==   total heap usage: 1 allocs, 1 frees, 12 bytes allocated
==6812==
==6812== All heap blocks were freed -- no leaks are possible
==6812==
==6812== For lists of detcted and suppressed errors, rerun with: -s
==6812== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

although Valgrind is pretty arcane, it's still powerful and useful
because it detects mistakes that either the human, us, or the debugger can't found.
because it reads the memory usage.
___

# Garbage values

```c
int main(void)
{
	int scores[3];
	for (int i = 0; i < 3; i++)
	{
		printf("%i\n", scores[i]);
	}
}
```
we didn't initialize `scores`
but the code still going to compile.
but when we execute the program, we can see what so-called *garbage values*:
```
// result
68476128
32765
0
```

since we didn't assign values to `scores` by ourselves
who know what are in those addresses.
it could be some remnants of past ints, chars, or anything, since the computer might been doing some other things using those memory addresses.

this can be a pretty serious problem actually.
As one of these variables in a software is not initialized, all of the sudden users, maybe people on the internet if it's a web application, could suddenly see the contents of someone else's memory.
Maybe that value in the address is someone's password that had been previously typed in, or something like a credit card number that previously typed in.

essentially this is how hackers find ways to exploit systems too.

although there are different defense mechanisms in place to make it not so likely to happen, initializing variables is just always a good practice.

also it's okay to not assign value to variables initially, so long as we eventually do.

ever think of how a computer identify garbage value and non-garabage value?
`malloc()` can keep track of the addresses of the memory from the heap that it handed to us
the compiler, similarly, keep track of the addresses of the memory from the stack
___

# pointer with garbage value

now with the understand on garbage value, take a look at this code.
```c
int main(void)
{
	int *x;
	int *y;
	
	x = malloc(sizeof(int));
	
	*x = 42;
	*y = 13;
	
	y = x;
	*y = 13;
}
```

what are the problems in this code?
1. `*y = 13;` won't work
   * because we didn't assign value to pointer `y`. what is inside it is a garbage value.
   * later when we do `*y = 13;`, the computer is going to a random address, since `y` has a grabage value and assign the value `13` there.
   * it is likely that the computer can't control that random address, and cause [[CS50x 4-1 Memory#power and danger of pointers|segmentation fault]].
2. we didn't check if `malloc()` for `x` is successful
___

# swap()

let say we have a function to swap the values of 2 variables.
here is how the function may look like:
```c
void swap(int a, int b)
{
	int tmp = a;
	a = b;
	b = tmp;
}
```
it seems perfect. so let use them:
```c
int main(void)
{
	int x = 1;
	int y = 2;
	printf("x is %i, y is %i\n", x, y);
	swap(x, y);
	printf("x is %i, y is %i\n", x, y);
}

// result
x is 1, y is 2
x is 1, y is 2
```

why `x` and `y` didn't get swapped?
because the function `swap(x, y)` is [[C - 7 Variable Scope#pass by value|pass by value]]
that means we are copying the value of `x` and `y` and pass the copies to the function `swap()`, and then do the swapping with those copies, but not `x` and `y`

so how do we solve this?
`a` and `b` should be the address of `x` and `y`, but not their values
so they are now int pointers, but not int
```c
void swap(int *a, int *b)
{
	int tmp = *a;
	*a = *b;
	*b = tmp;
}
```
and then how do we call `swap()`?
`swap(&x, &y);`
___

# memory layout
![[memory-layout-1.png|300]]
this is how the memory allocated in the computer's [[CS50x 2-3 Array#RAM - random access memory|RAM]].
1. when the computer [[CS50x 2-1 Compiling#Compile|compiles]] the code, it loads all of our program's zeros and ones (machine code) into just one big chunk of memroy at the top
2. then followed by [[C - 7 Variable Scope#Scope|global variable]], the variables declared outside of `main()` and other functions
3. then there comes the **heap**
	* any time we use `malloc()`, that memory ends up here
4. and finally another chunk of memory called the **stack**
	* any time we use local variables in a function, they end up here
	* what happen to the memory allocated for local variables when the function finished executing? they get returned to the computer and they become out of our control again.

and then if we use more and more heap, and more and more stack,
to a point that the 2 things barreling down the tracks at one another, it will not end well.
> [!stack overflow]
> occurs when we used a lot of `malloc()` but didn't `free()` enough of them, plus there are a lot of functions running at the same time using a lot of local variables
___

# `scanf()` and `get_string()`

now look at the old school way to prompt user for input, without using `get_string()`, `get_int()`, etc. that come with `<cs50.h>`
```c
int main(void)
{
	// prompt an int
	int x;
	printf("x: ");
	scanf("%i", &x);
	printf("x: %i\n", x);
}
```
this works fine.

but things start to get dangerous when it comes to string.
```c
int main(void)
{
	// prompt a string
	char *s;
	printf("s: ");
	scanf("%s", s);
	printf("s: %s\n", s);
}

// input
hello
// result
1. s: (null)
2. Segmentation fault (core dumped)
```

why it didn't work?
because we didn't make room for `s`. we need to do `char *s = malloc(4);` or `char s[4];`
but then if we input some word with 4 or more char, some weird things start to happen.

so how did `get_string()` works?
it uses `malloc()` to make room for every letter that the user type in
this essentially happen on every keystroke
___
