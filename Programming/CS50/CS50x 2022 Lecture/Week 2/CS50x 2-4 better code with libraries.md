# loop design

say we want a program prompts the user for input and then print the same thing:
```c
#include <stdio.h>

int main(void)
{
	string s = get_string("Input:  ");
	printf("Output: ");
	for (int i = 0; i < strlen(s); i++)
	{
		printf("%c", s[i]);
	}
	printf("\n");
}
```
this code is not optimal, why?
remember how [[C - 5 Loops#for loops|for loop]] works?

in this case the loop checks and runs `strlen(s)` every time 1 loop is done.
but the answer to that is just always 3.
we don't need it to run multiple times. but 1 time is enough.

so use this instead
```c
int n = strlen(s);
for (int i = 0; i < n; i++)
{
	printf("%c", s[i]);
}

// or this is just the same
for (int i = 0, n = strlen(s); i < n; i++)
```
___

# benefit from libraries

how about we want it to print all input letters in uppercase?
surely we can write the logic ourselves by something like:
```c
for (int i = 0, n = strlen(s); i < n; i++)
{
	if (s[i] >= 'a' && s[i] <= 'z')
	{
		printf("%c", s[i] - 32);
	}
	else
	{
		printf("%c", s[i]);
	}
}
```

or we can just use a function `islower()` and `toupper()` from a library `<ctype.h>`
```c
if (islower(s[i]))
{
	printf("%c", toupper(s[i]));
}
else
{
	printf("%c", s[i]);
}
```

`islower()` retuns an int.
* 0 means the argument is not a lower case letter
* any number that is not 0 means that it's upper case

but how come `islower(s[i])` return `true` or `false` if it is actually returning `int`?
it is just how *Boolean* works.
* anything that is 0 means `false`
* anything that is non-zero means `true`

so we can do `if (islower(s[i]) != 0)`
but `if (islower(s[i]))` is just more common

but actually `toupper()` itself will check if its input is a lower case or a upper case
so we don't even need the check in our code.
our code now is reduced to this:
```c
for (int i = 0, n = strlen(s); i < n; i++)
{
	printf("%c", toupper(s[i]));
}
```

so read documentation first before writing something that feel basic or useful, that is likely to be already written by someone else and put into some libraries
___
