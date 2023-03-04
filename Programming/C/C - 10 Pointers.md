pointers provide an alternative way to pass data between functions
* other than [[C - 7 Variable Scope#pass by value|passing by value]], which only pass a **copy** of data
* pointers allow us to pass the actual variable itself
	* so that we can change its value
	* [[#Dereferencing]]
* see [[CS50x 4-3 Playing with Memory#swap|swap]]
___

# Computer memory

* Files live on our disk drive
	* hard disk drive, or HDD
	* solid-state drive, or SSD

Disk drives are just storage space. Data in drives won't go missing after we turn off and reboot our systems. But data in RAM will get destoryed when we turn off out computers.

And we can't not directly work on or change the value in drives, we manipulate data by moving them to or creating them in [[CS50x 2-3 Array#RAM - random access memory|RAM]], and we do so by executing a program to do something with the data.

Memory is basically a huge array of 8-bit wide or 1-byte wide bytes.
Much like an array, every element in memory has an address. With these addresses, we can do so-called *random access*[^1] on the array.

[^1]: it means we don't need to iterate through the whole memory starting from the beginning to find what we want, but just need to have the computer to look for a specific address
___

# how memory are allocated?

* when we use `char c = 'H';`
   a byte in memory some where is assigned a value `'H'`.
* later when we use `int speedlimit = 65;`
   4 bytes of memory in some locations together are assigned a value `65`
( for how many space that a data type takes, see [[C - 0 cheat sheet#types|here]] )

remember `H` and `65` are actually in binary. they are the *binary representation* of `H` and `65`.
also the computer is not going to treat the 4 bytes of `65` as 1 4-byte chunk, instead it is still 4 1-byte chunks to the computer
also there is something called *endianness* that explain more about this story

* finally what if we use one more thing: `string surname = "Lloyd";`
   it's 6 bytes are taken from memory, since [[CS50x 2-3 Array#NUL|1 extra byte]] for char `NUL`.

also their locations may not be next to each other even though the 2 lines of code are right next to each other.
most systems are designed to allocate value at addresses, that with the index numbers that are multiples of 4
___

# Pointers are just Addresses
> Pointers are addresses to locations in memory where variables live.

1. what `int k;` does?
creating an int variable, named `k`

2. what `k = 5;` does?
we assign a value `5` to the variable `k`

3. what `int *pk;` does?
creating *something that probably related to type int*, named `pk`

4. what `pk = &k;` does?
`pk` is assigned the address of `k`, = the location where `k` lives in memory
`pk` gives us the information we need to find `k`
`pk` has an arrow pointing to `k`

see a more detailed explaination [[CS50x 4-1 Memory#Pointers|here]].

**pointer**
* a data item
	* value: a memory address
	* type: describes data type at that memory address
		* what's is in the address that it's pointing to. is it an int, float, bool, etc.?
* allows us to pass variables between functions, but not copies of variables
	* if we know exactly where in memory to find a variable, we can just go to that memory location and manipulate that the variable directly

**analogy**
let say the task is to update a notebook.
the way that a pass by value function would do is:
1. first take the notebook from its owner to a copy store to make a copy
2. do all the changes on the copy
3. pass the copy to its owner. and it's up to the owner to decide whether or not to update the original notebook.
but the way a function does that with pointers is like:
1. take the notebook from its owner
2. update it
3. give it back
___

# `NULL` pointer

the simplest pointer in C, is the `NULL` pointer
which points to nothing

do **always** set a pointer to `NULL` if it is not set to be pointing to something meaningful right after it is declared.
> [!info]
> if we just do declaring a pointer, but didn't assign a value to it, we're just leaving the value as it is. and that value is rarely `NULL`. see [[CS50x 4-3 Playing with Memory#Garbage values|garbage values]].

we can check whether a pointer is `NULL` using the [[C - 3 operators#^0e156e|equality operator]] `==`
___

# creating pointer

we can create pointer variable by doing something like:
1. `char *s = NULL;`
2. `&x`, where `x` is an int variable
	* create a *pointer-to-int* whose value is the address of `x`

how about `int* px, py, pz;`?
this is actually creating a int pointer `px`, and 2 int `py` and `pz`
if we want all 3 of them are *pointer-to-int*, it should be `int *px, *py, *pz;`
___

# array and pointer

okay, so what will `&arr[i]` do, assuming `arr` is an array of doubles?
create a *pointer-to-double* who value is the address of the `arr[i]`
so it's the same as `&(arr + i)`. see [[CS50x 4-2 string and address#pointer arithmetic|pointer arithmetic]].
because an array's name itself is a pointer to the first element of the array

that is why when we pass in array as function argument, it's not pass by value
we actually can change the array variable that is passed in
since what we are changing is the value in a memory location that the array points to instead of the value of a copy
> [!array as function parameter]
> `int add(int[] arr)`
> 1. the function copy the pointer `arr`
> 2. if we do something like `arr[i]` in the function, although the array, or pointer, is a copy, the address that it points to is the same as the original array that is passed in
> 3. therefore we can change the value at that address
___

# Dereferencing

why do we care about address?
so we can do **dereferencing**
* we change the value at a specific memory address
* but not the value of a copy that is copied from that memory address
* *we go to the reference and change the value there*

if we have a *pointer-to-char* called `pc`,
`*pc` is the data that lives at the memory address pointed from `pc`
* `*` is the **dereference operator**
* it **goes to** the reference, that is the data at that memory location

what if we try to dereference a pointer whose value is `NULL`?
we suffer from [[CS50x 4-1 Memory#^b94d99|Segmentation fault]]
but that is a good thing, because otherwise the program is going to do something crazy

say if we declared a pointer, we didn't set it to `NULL` but some time later we some how try to dereference it.
since that pointer may contains a [[CS50x 4-3 Playing with Memory#Garbage values|garbage value]], we may get to a memory address that we aren't intended to and even change its value.
it may just be better encounter the Segmentation fault and crash and quit the program,
rather than break another program or function or anything.
___
