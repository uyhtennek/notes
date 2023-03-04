# clang
[[Linux Commands explained#^943345|how to use clang]]

we usually use the `make` command to compile files.
but it's not the compiler. it's just a command that help us compile files.

`clang` is the compiler.
but different from `make`, we can't name the output file just by `clang`.
(the default name is *a.out*, stands for *assembler output*)
we need `clang -o` to name the output file

however, if we did use any function that is not defined in our code, `get_string()` for example, even we did `#include <cs50.h>`, `clang` compiler still will complain about the **undefined reference to `get_string`**
```bash
/usr/bin/ld: /tmp/hello-d17dfc.o: in function `main':
hello.c:(.text+0x19): undefined reference to `get_string'
clang: error: linker command failed with exit code 1 (user -v to see invocation)
```

Why is that?
___

# Library & header file

By doing `#include <cs50.h>`, it just teaches the compiler that the function `get_string()` and the like will exist.
But the function actually doesn't exist in our code. It is in the `cs50` library, which is compiled and put in to some where in the operating system.

1. Someone else wrote the function `get_string()` in **source code**, as well as other `cs50` functions
2. All the functions are compiled
	* So a library, as well as its functions, some how exist some where in our operating system
	* and they exist in the form of **machine code**
3. When we write our code, we need to include the header file, by doing `#include <cs50.h>`
	* `<cs50.h>` is the *header file*, to the *`cs50` library*
	* it makes the compiler some how *trust* that the function `get_string()`, as well as other functions included in `<cs50.h>`, exist in our code
	* but `clang` still can't find `get_string()`
4. we need to use `clang -o <name> <c file> -lcs50` to **link** the `cs50` library to our code
	* take the `math` library as another example, we need to use `clang -o <name> <c file> -lm`

**important:** functions exist in library, as machine code, not in header file


so we usually just stick with the `make` command to save us some trouble

but how come we don't need something like `-lstdio` to link the `stdio` library?
that's because *standard I/O* is so standard that it is built-in to C language and enabled by default
`cs50` library is not built-in to C
`math` library is built-in to C, but not enabled by default. It's just better performance because we don't need our computer to load it if our program don't need them.
But for `stdio`, we will load it anyway. because it is likely needed in most cases.
___

# "Compile"
actually it includes **4 distinct actions**

example
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	string name = get_string("What's your name? ");
	printf("hello, %s\n", name);
}
```
and it's important to know compiler, `clang` in this case, read code **top-to-bottom, left-to-right**


1. **Preprocessing**

The top paragraph of any source code is almost always the `#include <stdio.h>` and the like
`#` is called *preprocessor directives*, it tells the compiler to treat the following stuffs differently

remember how *prototype* can let us use the function before we define it?
([[CS50x 1-4 function in C#declaring a function]])
essentially those `#include <stdio.h>` are doing the same thing
but this time, the function is not inside our code, but some where else

for example, `cs50.h` is installed some where in the cloud's hard drive, that we access via VS Code
inside `cs50.h`, there is a line `string get_string(string prompt);` and a bunch of others

rather than copy pasting those prototypes, we just need to `#include <cs50.h>` 
then when we use the function `get_string()`, the compiler knows it's come from `<cs50.h>`

`printf()` in `<stdio.h>` works the same
but it's prototype looks a bit different: `int printf(string format, ...);`
the `...` represents that the function can take 1, 2, 3 or more arguments

Recap:
> Preprocessing
> 1. read `#include <header file>`
> 2. find those files on the hard drive
> 3. do the equivalent of copying and pasting the prototype in those files to the top of our code

now the code looks something like this in the computer's memory, or RAM:
```c
...
string get_string(string prompt);
int printf(string format, ...);
...

int main(void)
{
	string name = get_string("What's your name? ");
	printf("hello, %s\n", name);
}
```


2. **Compiling**

it just turns our C code to something looks like:
```
...
main:                           # @main
	.cfi_startproc
# BB#0:
	pushq    %rbp
.Ltmp0:
	.cfi_def_cfa_offset 16
.Ltmp1:
	.cfi_offset %rbp, -16
	movq    %rsp, %rbp
.Ltmp2:
	.cfi_def_cfa_register %rbp
	...
	...
	callq get_string
	...
	...
	callq printf
	...
```

fun fact:
if we take the CS50 class a few decades ago,
C didn't exist yet, so we would be learning this

it is *assembly language*
this is the lowest level code, before actual 0s and 1s
so PC, Mac, phone, etc, all devices use those code

although most of it is cryptic, we still can see some relationship to the C code that we wrote
given there are keywords like `main`, `get_string`, `printf`

for those `pushq`, `movq`, `subq`, `xorl`, `movl`, etc.
they are *computer instructions* or *assembly instructions*

at the end of the day, computers only understand really basic instructions
like addition, subtraction, division, multiplication, move into memory, load from memory, print something to the screen, etc.
that is what those *computer instructions* all about

the program will feed them into CPU,
then CPU do the corresponding actions
the outcome of doing those actions, long story short, is printing "hello, world!" on the screen

additionally, each type of CPUs has their own instructions set
that is why we can't take a program that was sold for a Windows computer and run it on a Mac, or vice versa. because the instructions are different.
for Microsoft or any other company, who write the program, need to write the code in one language and compile it twice. one for Windows and one for Mac.


3. **Assembling**

it turns the *assembly language* into 0s and 1s
```
011111101000011010100110001000110
000000010000001000000100000000010
000000000000000000000000000000000
000000000000000000000000000000000
000000000000011111100000000000000
...
```


4. **Linking**

the computer still don't know how about the actual `get_string()` function.
its prototype though, is in `<cs50.h>`, which is a header file
where is the actual implementation of `get_string()`?
`cs50.c`
same goes for `printf()` in `stdio`

after our code `hello.c` get turned into 0s and 1s
it is combined with `cs50.c` also in 0s and 1s
then the same goes to `stdio.c`

the **linking** phase just links the 3 paragraphs of 0s and 1s together,
into one single file called `hello` or `a.out` or whatever


**more notes**
* `cs50.c` = `cs50` library
* although `cs50.c` is a seperate file, `cs50.h` is included in our `hello.c`
	* in the form of prototype, and actually all its prototypes are included
	* hence included in the 0s and 1s paragraph that belong to `hello.c`
* order with libraries sometimes matter
___
