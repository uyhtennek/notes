> [!info]
> Python has a theme of to make code to be as readable as possible
> [Python Documentation](https://docs.python.org/3/)
___

# hello, world

```c
printf("hello, world\n");
```

```python
print("hello, world");
```

notice any difference?
1. `print()` instead of `printf()`
2. no semicolon `;`
3. no `\n`
    in Python, with `print()`, we can the new line for free, since it's sort of a common case
___

# `print()`

```c
string answer = get_string("What's your name? ");
printf("hello, %s\n", answer);
```

```python
answer = get_string("What's your name? ")
print("hello, " + answer)
```

again, any difference?
1. no type
	* it doesn't mean it don't exist anymore, data types are still there in Python. just we the programmer don't have to bother telling the computer what types we're using.
    * Python is smart enough to figure that out from context
2. `get_string()` this time comes from a Python version of the CS50 library
3. `"hello, " + answer`
	* adding, or appending, `answer` to the string `hello, `
	* in the form of joining them together

instead of typing `print("hello, " + answer)`,
`print(f"hello, {answer}")` is the equivalent
* this is actually a relatively new feature of Python
* `""` and `f""` are different, `f""` is what Python would call a *format string*, or *Fstring*
	* it indicates Python to *interpolate* everything inside a curly braces `{}` inside of the string
	* it means substitute the value of any variable inside the curly braces `{}`
	* basically `{answer}` will print the value of `answer`
	* what happen if we forgot the curly braces `{}`? it'll print `hello, answer`.

these kind of format can make our code a little tighter and a little more succinct
___

# declare a variable

```c
int counter = 0;
```

```python
counter = 0
```

no need to write the type, no semi-colon, same as before
also the single equal sign `=`, still behave the same as in C
___

# increment

```c
counter = counter + 1;
counter += 1;
counter++;
counter--;
```

```python
counter = counter + 1
counter += 1
# no counter++
# nor counter--
```

seems like the community just don't like the idea of having `++` or `--` in Python
___

# `if` condition

```c
if (x < y)
{
	printf("x is less than y\n");
}
```

```python
if x < y:
	print("x is less than y")
```

again, the differences:
1. no curly braces `{}`
2. use colon `:` instead
3. no parentheses `()` around the condition
	* why not? it's just simplier to type.
	* we still need them if we want to combine conditions though, like doing if this and that, or if this or that

also so long as we indent subsequent lines, that implies everything below the condition should be executed, until we un-indent.
so *indentation* in Python is important, although in C a poor styled code can still compile and run.
___

# `if-else` condition

```c
if (x < y)
{
	printf("x is less than y\n");
}
else
{
	printf("x is not less than y\n");
}
```

```python
if x < y:
	print("x is less than y")
else
	print("x is not less than y")
```
___

# `if-else-if-else` condition

```c
if (x < y)
{
	printf("x is less than y\n");
}
else if (x > y)
{
	printf("x is greater than y\n");
}
else
{
	printf("x is equal to y\n");
}
```

```python
if x < y:
	print("x is less than y")
elif x > y:
	print("x is greater than y")
else:
	print("x is equal to y")
```

1. not `else if`, but `elif`
___

# loop forever

```c
while (true)
{
	printf("meow\n");
}
```

```python
while True:
	print("meow")
```

1. parentheses `()` are gone
2. colon `:` and indentation are there
3. no semi-colon `;`
4. `True` is capitalized
	* just because
	* same goes to `False`
___

# loop 3 times

```c
int i = 0;
while (i < 3)
{
	printf("meow\n");
	i++;
}
// or
for (int i = 0; i < 3; i++)
{
	printf("meow\n");
}
```

```python
i = 0
while i < 3:
	print("meow")
	i += 1
# or
for i in [0, 1, 2]:
	print("meow")
```

the idea is the same, just following Python syntax
also the for loop is meant to read a little more like in English
* the square brackets `[]` represent an array, now to be called a *list* in Python
* first `i` is set to `0`, then it's set to `1`, then it's set to `2`, then the loop finish

but what if we don't know how many times a loop should be iterate in advance, then we can't do hard code looping like this.
also are we suppose to type all the way until `100` if we do need to iterate 100 times?

in Python, there is a function, or technically a type, `range()`
```python
for i in range(3):
	print("meow")
```
it essentially magically gives us back a range of values from `0` on up to but not through the value
* `range(3)` gives `[0, 1, 2]`
* also it can take not just 1 argument, so we can customize its behavior a bit
	* like counting from `10` instead of `0`
	* or like incrementing by not just `1` but `2`
___

# data types

| C | Python | more Python |
| :-: | :-: | :-: |
| bool | bool | range |
| char | | list |
| double | | tuple |
| float | float | dict |
| int | int | set |
| long | | |
| string | str | |
| ... | ... | ... |

unlike `string` in C which is in the `CS50` library,
`str` in Python is a built-in data type, but `char` doesn't exist in Python any more
Python has no data type for individual characters
also in Python we can use either single quote `''` or double quote `""` for a str

`int, float` in Python don't need the corresponding `long` and `double`
because Python makes them can get as big as we want
* [[CS50x 1-3 Data types limitation & Programming mindsets#overflow explained|integer overflow]] is no longer an issue in Python
* [[CS50x 1-3 Data types limitation & Programming mindsets#^605b9c|floating point imprecision]] unfortunately is still a problem in Python

then there are more data types that Python offers
* `range`
* `list`
* `tuple`
	* eg. `x, y`, *latitude*, *longitude*
* `dict`
	* shorts for *dictionaries*
	* store *keys* and *values*, much like [[CS50x 5-4 Hash Table|hash tables]]
* `set` ^b87f09
	* it filters out duplicates when we throw to it a whole bunch of numbers or words or etc.
___

# `CS50` library

| C | Python |
| --- | --- |
| `get_char()` | |
| `get_double()` | |
| `get_float()` | `get_float()` |
| `get_int()` | `get_int()` |
| `get_long()` | |
| `get_string()` | `get_string()` |
| ... | ... |

```c
#include <cs50.h>
```

```python
import cs50
```

Python supports libraries too, and there aren't [[CS50x 2-1 Compiling#Library header file|header files]] anymore
we just use the name of the library in Python

or if we want to be more precise, not just import the whole thing but just some certain functions
to make it easier and faster to compile, typically when the library is huge and has many functions
we can do
```python
from cs50 import get_float
from cs50 import get_int
from cs50 import get_string
# or
from cs50 import get_float, get_int, get_string
```
___

# Comment

comment in Python starts with `#` instead of `//` now
`//` is a kind of division operator in Python, see [[CS50x 6-2 C and Python#^b1d172|here]]

```python
# This is a comment
some code
```

convention is we would use a full sentense, so first letter is capitalized
another convention is we would put the comment above the code, rather than next to it
___
