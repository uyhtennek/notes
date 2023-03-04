# Static memory allocation

we used something like `int *px = &x;` to point `px` at address of `x`
but this is *static*.
all the memory that we use for the program, is already set up when the program begins.

but sometimes we need to create and allocate memory on the fly.
maybe we are asking the user to give a number that will determine how many linked list object to generate.
in that case we need to get new memory in our program while it is already running.
___

# stack and heap

^da438a

recall how [[CS50x 4-3 Playing with Memory#memory layout|memory is distributed]]

* variables that is created *statically*, that is created when the a function starts, memory allocated for them come from the **stack**
* variables that is created *dynamically*, that is created while a function is running, that is created through `malloc()`, memory allocated for them come from the **heap**

![[memory-layout-2.png|500]]
the main message from this diagram is that:
* the **stack** for statically declared memory grows **up**
* the **heap** for dynamically declared memory grows **down**

how do we access memory on the heap?
we use the function `malloc()`
* it comes from `<stdlib.h>` library
* we pass in how many **byte** of memory that we want, as the argument
	* if we want an int, we should use `malloc(4)`
	* if it's char, use `malloc(1)`
	* if it's double, use `malloc(8)`
	* etc.
* it'll find some chunks of memory with the size that we want
* it returns a pointer to the memory it just found
	* if it can't find an available memory, it returns back `NULL` instead
		* so **always check for `NULL`** after `malloc()`, to prevent ourselves from [[C - 10 Pointers#Dereferencing|dereferencing]] a `NULL` pointer
		* usually it happens when the system runs out of memory or there is other catastrophic (災難性的) failure that we can't predict

> [!example]
> ```c
> // statically obtain an integer
> int x;
>
> // dynamically obtain an integer
> int *px = malloc(sizeof(int));
> 
> //------------------------------------------
> 
> // get an integer from the user
> int x = get_int();
>
> // array of floats on the stack
> float stack_array[x];  // this is not allowed in the old C though
>
> // array of floats on the heap
> float *heap_array = malloc(x * sizeof(float));
> ```
___

# memory leak
it means some memory is not released back to the system when a function is done. see [[CS50x 4-3 Playing with Memory#Valgrind|Valgrind]].
it is bad because it makes our system slow.

eg.
some browsers are notorious for not freeing memory. if we keep them open for days, the computer slows down significantly.

so when we finish working with the *dynamically-allocated memory*, we should `free()` it

memory from [[#^da438a|stack]] don't need to worry about this
because they can released automatically once the function finished executing

in addition, once our program is finished, that is `main()` is finished or get terminated, all memory related to our program will get freed.
but still it is a better practice to free memory by explicitly calling `free()`

> [!3 golden rules:]
> 1. Every block of memory that we `malloc()` must subsequently be `free()`
> 2. Only memory that you `malloc()` should be `free()`
>     we don't want to free a statically declared variable, since the system is expecting to take care of it by itself
> 3. Do not `free()` a block of memory more than once
>     it's called *double free*, it's like a *inverse memory leak*
>     we tricked the system to think it has more memory than it actually has
___
