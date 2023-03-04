# why do we care about addresses?

actually, usually we don't care.
___

# string and char

`string s = "HI!";`
remember what would it looks like in memory? (see [[CS50x 2-3 Array#NUL|NUL]])
`H, I, !, \0`
and we can access each char with `s[0]`, `s[1]`, `s[2]`, `s[3]`

let say `s[0] - H` is at the address `0x123`
what are the address of `I`, `!`, `\0`?
```
s[0] - H - 0x123
s[1] - I - 0x124
s[2] - ! - 0x125
s[3] - \0 - 0x126
```

but where is `s`?
`s` is a variable right? isn't a variable would take up space in memory?
yes. it does. because `s` is a [[CS50x 4-1 Memory#Pointers|pointer]].
pointing to `0x123` or `s[0]`.
so `s` tells the computer where is the beginning of the string, and `\0` tells where is the end.
> [!STRING]
> string, is a pointer, storing the address of the first character in a char array.

now, looking back, what's the true form of `string s = "HI!";` ?
`char *s = "HI!";`
since all `s` does, is pointing to the first character in the char array `"HI!"`
in addition, notice that we always trust there will always be a NUL char in the end of a char array.

now let us try something:
```c
int main(void)
{
	string s = "HI!";
	char c = s[0];
	char *p = &c;
	
	printf("%p\n", s);
	printf("%p\n", p);
}

// result
0x402004
0x7ffd4227fdd7
```
both `s` and `p` pointing to the first char in the array `"HI!"`, right?
why are the address different?

well. they don't point to the same location.
`p` is actually the address of `c`. but `c` is another char variable that we created.
so where `s` is pointing to, is the address of first char in `"HI!"` (or simply put it as address of `"HI!"`). but where `p` is pointing to, is `c`, which is in another location, although we did assign a value `H` to `c`.

so remember everytime we declared a variable. the computer will make new room in memory for the variable.

now, in addition, we added a line `char *p = &s[0];` and then print `p`. what's its value?
it's gonna be the same address as `s`. in the above case it'll be `0x402004`.

lastly, what is `s`? it points to the first character of the char array.
so it's actually same as `&s[0]`.

> [!didn't figure out what's going on?]
> 1. we have a char array (= string), `"HI!"`, placed some where in the memory.
>     remember the `NUL` char is placed here already at the end of the char array
>     `"HI!"` = `H, I, !, \0`
> 
> 2. `string s` is actually `char* s`, so `s` is actually a pointer type variable
>     and `s` didn't store the whole char array `"HI!"`
>     but only the address of the first character of `"HI!"`
> 
> 3. when we format a string using `%s`, it knows it should keep printing the char, starting from `s`, = `&s[0]`, that points to the starting address of `"HI!"`, and then keep printing all the way until it reaches `\0`

more on string, you may notice that strings are usually stored in a very low address compared to other data type like int or float.
that is because strings are often stored in a different part of memory for efficiency sake.

wonder how a string data type is created in code?
first recall what is a [[CS50x 3-3 Data Structure#Custom Data Structure|struct]]. an example would be:
```c
typedef struct
{
	string name;
	string number;
}
person;
```
in this example we define a data type named `person` and encapulate 2 strings `name` and `number` in a `person`.
now how do we define string type?
it's a pointer to a char with a name `string`. so this is all it takes to create a string data type.
```c
typedef char *string;
```
___

# need of `strcmp()`

and looking back from now, do you see why we can't [[C - 3 operators#string comparison|compare 2 strings]] with `==` ?
because it would be comparing the addresses but not the strings at those addresses.
```c
int main(void)
{
	string s = get_string("s: ");
	string t = get_string("t: ");  // convention is what follow s is t
	
	if (s == t)
	{
		printf("Same.\n");
	}
	else
	{
		printf("Different.\n");
	}
}

// input
s: hi!
t: hi!
//result
Different.
```
by now we understand that `string` is actually `char *`
by `s == t` we're comparing the address of the first char of `s` and `t`
although both `s` and `t` have the same value, they might end up in different locations in memory
since `get_string()` would make a copy of the input string, store it in memory some where, finally returns a `char *`, which is the address of the first char

> [!line: string s = "hi!";]
> 1. `h, i, !, \0` are stored back-to-back-to-back some where in memory
> 2. 8 byte of memory is assigned to a pointer type variable `s`
> 3. `s` stores the address of `h`, since it's the first char

what is the solution?
use `strcmp()`. which probably is a loop iterating through each char in string.
if it catches different it returns a positvie or negative number.
if it doesn't, it retures 0.
___

# need of `strcpy()`

how about if we do this?
```c
int main(void)
{
	string s = get_string("s: ");
	string t = s;
	
	t[0] = toupper(t[0]); // toupper() makes the input char become upper case
	
	printf("s: %s\n", s);
	printf("t: %s\n", t);
}

// input
hi!
// result
s: Hi!
t: Hi!
```
why `s` becomes `Hi!`?

as we know, string and array are addresses of the actual value but not the actual value per se,
what `string t = s` is copying the address value from `s` to `t`, intead of copying the value `hi!`
so `s` and `t` are both pointing to the same address
> [!line: string t = s;]
> 1. user input, `"hi!"` or `h, i, !, \0` is stored in memory some where
> 2. 8 bytes of memory are assigned to `s` to store the address of `h` (let say it's `0x123`)
> 3. another 8 bytpes of memory are assigned to `t`
> 4. we copied the value of `s`, which is the address `0x123`, to `t`

so what is the solution?
use `strcpy()`.
the catch is that it takes in, as arguments, more than just the source string that we want to copy, but also the the address of a chunk of memory that we want to copy the string to

how to create new chunks of memory though?
see the [[CS50x 4-3 Playing with Memory#Dynamic memory allocation|next page]].
___

# pointer arithmetic
> that means treating pointers as numbers, since they are.
> that means doing math on pointers.

how do we print out a single char in a string normally?
```c
int main(void)
{
	string s = "HI!";
	// or char *s = "HI!";
	printf("%c\n", s[0]);
	printf("%c\n", s[1]);
	printf("%c\n", s[2]);
	printf("%c\n", s[3]);
}

// result
H
I
!

```

now what are the equivalent of `s[0]`, `s[1]`, etc.?
```c
int main(void)
{
	char *s = "HI!";
	printf("%c\n", *s);       // s is the address of the first char in a string
	printf("%c\n", *(s + 1)); // 'I' is 1 byte after s
	printf("%c\n", *(s + 2));
}
```
this is how square bracket `[]` works in the lower level.

fairly the same goes for array of other types.
```c
int main(void)
{
	int numbers[] = { 4, 6, 8, 2, 7, 5, 0 };
	
	printf("%i\n", *numbers);
	printf("%i\n", *(numbers + 1));
	printf("%i\n", *(numbers + 2));
	printf("%i\n", *(numbers + 3));
	printf("%i\n", *(numbers + 4));
	printf("%i\n", *(numbers + 5));
	printf("%i\n", *(numbers + 6));
}
```
but wait, isn't an int takes 4 bytes in memory?
it should be `*(number + 4)`, `*(number + 8)`, `*(number + 12)`, etc. to get the 2nd, 3rd, 4th belongings?
actually the computer will take care of this detail for us. it is called *pointer arithmetic*.
`*(numbers + 1)` means go 1 more piece of data, instead of 1 byte. no matter what the data type is, the compiler knows how many byte to skip because it knows what kind of data type that we're talking about.
___

# array / pointer / adjacent addresses

notice that we declared an array `numbers[]`, and later we can treat `numbers` as a pointer by using `*numbers`.
although we didn't do anything special when declaring `numbers`, not like declaring `*s`.

it turns out that an array can be treated as pointer which points to the address of the first element in that array.
___
