# Color

| | R | G | B | hash |
| --- | :-: | :-: | :-: | :-: |
| black | 0 | 0 | 0 | #000000 |
| white | 255 | 255 | 255 | \#FFFFFF |
| red | 255 | 0 | 0 | \#FF0000 |
| green | 0 | 255 | 0 | \#00FF00 |
| blue | 0 | 0 | 255 | \#0000FF |

see the pattern?
what is FF? `FF = 255`
___

# Hexadecimal

let think back about base system
**[[CS50x 0-1 Information in computer explained#Why use binary digit aka bit|Binary]]**:
	- 2 digits
	- `0 and 1`
Decimal:
	- 10 digits
	- `0, 1, 2, 3, 4, 5, 6, 7, 8, 9`

Here comes a new one.
Hexadecimal:
	- 16 digits
	- `0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F`
	- aka. base-16
	- $\#_1\#_2$
	  = decimal: $(16\times\#_1) + (\#_2)$

| decimal | hexadecimal | decimal | hexadecimal |
| :-: | :-: | :-: | :-: |
| 0 | 00 | 11 | 0B |
| 1 | 01 | 12 | 0C |
| 2 | 02 | 13 | 0D |
| 3 | 03 | 14 | 0E |
| 4 | 04 | 15 | 0F |
| 5 | 05 | 16 | 10 |
| 6 | 06 | 17 | 11 |
| 7 | 07 | 18 | 12 |
| 8 | 08 | 19 | 13 |
| 9 | 09 | ... | ... |
| 10 | 0A | 255 | FF |
[^1]: FF = 16 x 15 + 15 = 240 + 15 = 255
___

# Why hexadecimal?
what's the benefit of using hexadecimal?

in decimal, how many digits that we need in order to count to 255?
3.
how about hexadecimal?
2.
and then the bigger the number, the less digits we used compared to decimal.

does it save computer's memory? since we used fewer digits?
no. computer stores information using bit.
how many bits are required to store number 255?
8 bits. `11111111`.
how many bits are required to store number FF?
8 bits. `11111111`.

but now we can starting to see why hexadecimal is good.
* it's **convenient** to treat data in units of 4 or 8 bits
	* the 1st hex digit is representing the left half of `11111111`
	* the 2nd hex digit is representing the right half of `11111111`
* every 4 digits in binary can be groupped as 1 digit in hexadecimal
	* `01000110101000101011100100111101`
	* `0100 0110 1010 0010 1011 1001 0011 1101`
	* `0x46a2b93d`
then it's useful in counting memory

but how do we distinguish a hexadecimal number from decimal numbers?
`10` in decimal is 10. but `10` in hexadecimal is 16. it's confusing isn't it?

By convention, any time a `0x` comes before a number, that means this is in fact a hexadecimal number. eg. `0x10`
___

# Pointers
a pointer is a variable that stores the **address** of some value
= the location of that value = the **specific byte(s)** in which that value is stored

let say we initialized an int variable `n` with a value of 50.

first of all remember how many bytes typically do we use for an int?
4 bytes. 32 bits. [[C - 1 data types#int|see here.]] ^aa7741

so, somewhere in the computer's memory,
it's using 4 bytes to store the value 50 and named it `n`.

now how do we get the address of `n`?
`&n`
* `&` is the *address of* operator.
* it returns a number, known as the address of `n`

and what if we want to store the address of `n` in a variable?
`int *p = &n;`
* although `&n` gives a number, we are not going to store the number to an int variable, but the address of an int variable
* `*` tells the computer that `p` is not an int variable, but the address of some int variable (= a int pointer)
* `int *` is the type of the variable, and the type is an address of an int variable
	* it's okay to do `int* p` or `int * p` though, just `int *p` is the convention
* `p` is the name of this address variable
	* we access `p`'s value (= address of `n`) by just using `p`
* `*p` however, is the value at the address `p`, thus it's the value of `n`
	* `printf("%i\n", *p)` = `printf("%i\n", n)`
	* `*` in here is what so-called [[C - 10 Pointers#Dereferencing|dereference operator]], which means *go to that address*
	* note that by using `*p` the computer knows to read the address and stop at exactly 4 bytes
	   why? because we told the computer `p` is an address of an int, by `int *p`

* summary:
	* `*p`, go to the address `p`, it's something like `0x7ffcb4578e5c`
	* in address `p`, it is storing an int variable `n`, using typically [[#^aa7741|4 bytes]]
	* `n` or `*p` (or the binary contained in memory address `p`) have a value of `50`
	* `int *p` and `*p` and `p` are confusing but they are all different things

lastly how to print or format a pointer?
`printf("%p\n", p)` / `printf("%p\n", &n)`
* `%p` is a special format code for printing pointers or addresses
* result would look something like `0x7ffcb4578e5c`
* notice the result may not be the same every time we run the program. why?
	* it's just where the address of `n` was in that moment of time when the program is executed

oh. 1 more thing.
Do pointers have pointers?
Yes. We can do something like `int **p` or `&*p`. ( `&*p` gives the address of the int pointer `p` )
Luckily we don't have to do this often, unless we're working on 2 dimensional arrays

think of a pointer as an arrow.
when you get to the parts of memory which is a pointer, you see an arrow. Following along the arrow, eventually you get to see another variable.
similar to how a *forwarding address* in a regular mail post system works:
1. for example you moved from San Fransico to New York.
2. the Post Office put some kind of piece of paper in your old mailbox
3. it says something like the mail is forwarded to the new address
___

# power and danger of pointers

now we know how to take a look at memory. in fact, we can look around at all of the computer's memory, even at things that we didn't put there but maybe other programs did.

this could be a *security threat*.
although there are some defenses in compilers and in our operating systems that do hedge against this problem a little bit.
but it's still a frequent source of problems.
like *Stack overflow*, *heap overflow*, or generally *buffer overflow*.

eg. `Segmentation fault`.
it's an error saying that the program touched memory that it shouldn't have
like going "out of bounds" of a array
but it's *non-deterministic*, which means sometimes it will happen but sometimes it won't.
often depends on how far off the end of the array that we go. ^b94d99
___

# Size of a pointer

As mentioned above, `int *` is a type. it's a pointer, to an address of an int variable.
and `p` is the variable of `int *` type.
in which, it's a hexdecimal number, that represent the address of an int variable, `n`.

As we all know, every type has a defined size, = how many bits or bytes they will take from the memory. so now, what is the size of a pointer?

8 bytes.
And it's doesn't matter the pointer `p` is declared to be a pointer to an int variable, bool, char, float or even long. `p` is always 8 bytes.
why is that?

Size of a pointer used to be only 32 bits = 4 bytes, even 8 bits. when memory was still expensive.
how many numbers can we get from 32 bits? roughly 4 billions. 2 billions if we need negative.
4 billions bytes. that's about the total size of memory a computer could have back in the days.
so a 4-bytes number is enough to represent any address in a older computer.

nowadays, computers usually have 8 GB, 16 GB, even 32 GB of memory.
32 GB is roughly 32 billions bytes.
what if we use 4 bytes as the size of a pointer?
that gives us roughly 4 billions numbers to use.
we can use the numbers to represent 4 billions bytes out of a 32 GB memory. that would leave 28 billions bytes of memory untouchable from us.
how many bits do we need, to give us a number larger than or equal to 32 billions?
35 bits actually. but we make it 64 bits total, maybe for future proof. so we don't need to worry about running out of bit when pointing to memory, for many years.
___
