# Composition of a Function (in C)

`printf("hello, world\n");`
print something, but have to be string type, in this case it's `hello, world`
`f` stands for 'formatted'
`;` must be placed, at the end of line. it's a syntax.

notice that function also has both input and output:
arguments --> `function` --> return values

but how to we catch return values from a function?
`answer = get_string("What's your name? ")`

`=`  it's a assignment operator. it assigns something. it doesn't mean equality.
it's read from right-to-left,
so it means assign value on the right to the variable on the left

however the above example will not compile. the correct one shows as follow:
```c
string answer = get_string("What's your name? ");
// in C, the convention is to use all lowercase letters for variable names.
```

notice we need to declare what the type the variable `answer` is. in this case it's a `string`.
and this is how the computer knows which set of 0 and 1 are words, which set are numbers, or audio, and etc.

again, `;` must be there.

but what happen if we try storing a `string` value in a `int` variable?
apparently the code will not compile. the compiler will complain about it and give us error messages.
___
# Common programming knowledge

`\n`
why is it there?
well, it doesn't have to be there. the code can still compile and run.
it means create a new line.
so after the program run, CLI will get to a new line instead of stay on the same line.

can we do this?
```C
printf("hello, world
	   ");
```
no, at least in C.
long story short, double quotes `""` just have to be on the same line in C just because.
just remember what enter key usually does, is replaced by `\n` in C

now take at look at our new `hello.c`
```C
#include <stdio.h>

int main(void)
{
	string answer = get_string("What's your name? ");
	printf("hello, answer\n");
}
```
actually this has an error. so it won't compile.
(actually the compiler will give a bunch of error messages, although there is only but 1 mistake)
> Key take-away: not to get overwhelmed by the number of errors

but where is the error?
Before answering that, let's take a look at something else first.

What is `<stdio.h>`?
It's a library called standard I/O, where I/O stands for input and output.

And what is a library?
Many functions don't come with the language itself, but come with the library. So we need to load the library first to use the functions included in the library. Think of it as an extension.
One common example is `printf()` is a function comes from `<stdio.h>`.

Back to the error. It turns out there is no `get_string()` function in C.
In fact, it's in `<cs50.h>` library. So we just need to include it to solve the error.

Now the code compile. What happen when we run it? What is the output?
`hello, answer`

Why it prints `answer`, but not what we inputted?
because we didn't pass in a variable in `printf()`, we just passed in a string value which is `"hello, answer\n"`

How to fix it?
instead we use `printf("hello, %s\n", answer)`

`%s`
= place-holder
= format code
it tells the computer to place a string here because that is the type that we eventually will put in there

and then we place the `answer` variable after the double quotes

and that's why we use `printf()` with the `f`.
it formats its input, in this case `answer`, to the format that is declared with placeholders, in this case `%s` for `string`[^1].
[^1]: actually C doesn't has `string` data type, but has array of `char`
___
# Simple design concept
A
```C
string answer = get_string("What's your name? ");
printf("hello, %s\n", answer);
```
B
```C
printf("hello, %s\n", get_string("What's your name? "));
```

The 2 code will too compile, and will run perfectly and give the same result.
But which has a better design?

If A
1. It's nicer to read top to bottom.
2. If we later want to reuse value in `answer`. B doesn't give us that option.

If B
1. It's one less thing to think about
___
# header file

`.h` files are refered as header file. Think of them as a menu of functions.
but remember header file and library are not exactly the same thing.

Library: `cs50`, `std.io`
header file: `cs50.h`, `stdio.h`
___
