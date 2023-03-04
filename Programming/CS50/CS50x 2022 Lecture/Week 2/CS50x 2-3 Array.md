# types
* bool, char, double, float, int, long, string, ....
see [[C - 0 cheat sheet]]

**bool**
* for simplicity it uses the whole 1 byte, although it just need 1 bit, 1/8 of a byte
* to represent 0 or 1, false or true

**char**
* only have 1 byte = 8 bits
* that's why ASCII also only has 1 byte, but technically 7 bits, to store maximally 256 characters
* see [[CS50x 0-1 Information in computer explained#Letters in computer|Letters in computer]]

**int / long, float / double**
* int, float are 4 bytes
* long, double are 8 bytes

**string**
* it's an array of *char*, but we don't know how many *char* that will include
* it takes up a variable amount of memory space
___

# RAM - random access memory
![[RAM.png]]
this is how a RAM looks like
the important parts are those *black chips*
they are where the 0s and 1s are actually stored. Each of those stores some number of byte.
___

# Array

let say we want to have a program which calculate an average score, so we wrote `score.c`
```C
#include <stdio.h>

int main(void)
{
	int score1 = 72;
	int score2 = 73;
	int score3 = 33;
	
	printf("Average: %f\n", (score1 + score2 + score 3) / 3.0);
}
```
it works well. and we can make it work with `get_int()`.
so what is its problem?

Later in the semester we may have more subjects and more scores.
However in this program, the maximum number of score we can input is capped at 3.
Also this program is kind of coping and pasting code.
> Imagine doing this for a whole grade book for a class, we have score 4, 5, 6, 11, 12, 20, 30, a lot of variables. The code may still work, but it certainly will get ugly, and the logic in this code is certainly not good.

Luckily we have a data type that can store multiple values of the same data type in memory
It is **array**
* store multiples values of the same type *back-to-back-to-back* ( = contiguously )
* allow us to create memory for some number of a data type
* values stored are described using the same variable name, this is its purpose
* an array of 3 int takes the same amount as declaring 3 seperate int variables. array didn't save space for memory. its purpose is only eliminate repeating code.

```c
// declare a array of the size of 3 int
int scores[3];

// int    - the data type that the array is going to store
// scores - the name of the array
// [3]    - it tells the computer to create memory for 3 int
```

if we want to assign values to the items in `scores`:
```c
scores[0] = 72;
scores[1] = 73;
scores[2] = 33;
```
it uses *0 indexing*, which means to start counting at 0

`scores[0]` means "index into" the array `scores` to get or set the first item.

but now we still have the same problems as before. to improve it we do:
```c
for (int i = 0; i < 3; i++)
{
	scores[i] = get_int("Score: ");
}
```
also we can ask the program how many scores they need to input to make the whole program dynamic:
```c
#include <stdio.h>

int main(void)
{
	int n = get_int(How many scores? );
	int scores[n];
	
	for (int i = 0; i < n; i++)
	{
		scores[i] = get_int("Score: ");
	}
	
	printf("Average: %f\n", (score1 + score2 + score 3) / 3.0);
}
```
___

# type casting

```c
char c1 = 'H';
char c2 = 'I';
char c3 = '!';

printf("%c%c%c\n", c1, c2, c3);

// result
HI!


// what if we use:
printf("%i %i %i\n", c1, c2, c3);

// result
72 73 33    // they are the ASCII value of the char 'H', 'I' and '!'
```
it's because `printf()` is able to tolerate printing the `char` values as `int`

it's the same as we casting, or converting, `c1`, `c2`, `c3` to interger like this:
`printf(%i %i %i\n, (int) c1, (int) c2, (int) c3);`
(consider the compiler is doing implicit casting for us)

we can cast char to int, also float to int, int to float, etc.
sometimes they are equivalent, but other times it will lose information
eg. - taking a float to an int, will throw away everything after the deciaml point
___

# string & char
> `string` is an array of `char`

```c
string s = "HI!";
printf("%i %i %i\n", s[0], s[1], s[2]);

// result
72 73 33
```
___

# NUL

okay now we have a string `"HI!"`. now we declared another string `BYE!`.
they are put together in the memory, in 0s and 1s, back-to-back, right?
but how the computer where the string `"HI!"` ends and where the string `"BYE!"` begins?

no they are not placed back-to-back. between them is a `NUL` character
* it's represented by a special symbol `\0`, we use `\0` in code but not `NUL`
	* we would never write down `NUL` in code
* it's a shorthand notation for 8 0-bits, `00000000`
* *NUL* is the nickname
* this `NUL` character is placed at the end of the array automatically by using the double quotes `""`

so a string `"HI!"` actually has 4 char,
it implies that the computer is storing `"HI!"` using 4 bytes
```c
s[0] - 72
s[1] - 73
s[2] - 33
s[3] - 0    // the 1 extra byte that's sort of invisibly there
```

but why `char`, `int`, `float`, etc. don't need `NUL`?
think about how long is a `string`.
`char` is 1 byte. `int` and `float` are 4 bytes. how about a `string`?

we human know `"HI!"` contains 3 `char`, so it should be 3 byte.
but the computer don't know. a `string` can contains any number of `char`.

so we human defined a special character there for computers and tell them, when ever you read this `NUL` character, that is the end of a `string`


So we tell the computer to put a `NUL` at the end of a string.
But why not the beginning of a `string`?

No we can't do this, because it doesn't necessarily have another `string`, which has the character `NUL` at its beginning, coming next.
If not, computers still can't tell where a `string` ends.
___

# string.h

Now we know about `NUL`.
We can know how long is a string in code, by doing something like:
```c
int string_length(string str)
{
	int i = 0;
	while(str[i] != '\0')
	{
		i++;
	}
	return i; // then i is the length of str
}
```

Or we can `#include <string.h>` and then use a function called `strlen()`.
which does pretty much the same thing.
___

# extra: how to read documentation?
eg. [CS50 Manual Pages](https://manual.cs50.io/)

usually there are several titles in a page about a function / a data type / etc.

**SYNOPSIS**
what it needs to use the function.

**DESCRIPTION**
what the function does

**RETURN VALUE**
if any

**EXAMPLE**
example

so if we ever want a function that does some certain things.
the same function may already be written by some one and put into the documentations.
better to familiar ourselves with the options.
___
