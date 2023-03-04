# Compile in Python

Recall in C running a program is a 2 step process
1. use `make hello` to compile `hello.c`
    or `clang -o hello hello.c -lcs50`
2. run `./hello`

Moving on to Python, it becomes simplier
1. `python hello.py`
    it's just a convention to use a different file extension for a different language

also it jumps run in to the program after it is done compiling
`hello.py` is not only the name of the script, but also the name of the program

this is to say that Python is generally described as being *interpreted*, not *compiled*, it's just mean we get to skip the compilation step
* we used to write the code, compile the code into 0s and 1s, then finally run the 0s and 1s
* now instead we get to just *run the code*, without bother compiling it
	* well technically it still compile, but not to 0s and 1s, instead it compiles to something called *byte code*, which is just an intermediate step to turning the code into 0s and 1s
	* the benefit of byte code is that when we re-compile the code, it only re-compile the parts that is changed, but not the whole thing, so the process of recompiling is faster
* we pass the code to an *interpreter*, not a *compiler*
   the name of this *interpreter* is also happen to be `python`

the price is that things might be a little slower
since some time is spended on translating the code to 0s and 1s eventually

also, the idea of compiling code or interpreting code, is not native to the language itself
so we can have a interpreter for C or a compiler for Python, it's just nobody has ever done this
___

# Python Libraries
Python is good for its darn amount of libraries, that other people wrote

remember how to [[Bitmaps#^8a5fec|blur]] an image?
now let us write a `blur.py` that does that ^d898ac
```python
from PIL import Image, ImageFilter

before = Image.open("bridge.bmp")
after = before.filter(ImageFilter.BoxBlur(1))
after.save("out.bmp")
```
That's it.

notice that in the world of Python we would use the dot operator `.` a lot more
since Python has what's called *object-oriented programming*, or *OOP*
> [!object-oriented programming]
> 
> It just means functions can be embedded inside of variables, or more specifically inside of the data types themselves.
> 
> Recall in C there is a function `touppercase()` which uppercase any char passed in for us. If upper-casing chars is such a useful thing, what the world of Python does is it embeds the ability to uppercase any char, by treating the char as a struct in C.
> 
> Recall a struct encapsulate multiple types of values. In the world of object-oriented programming, we can encapsulate not just values, but also functionality.
> 
> Functions can now be inside of structs. But we now don't call them structs, but objects.
> Also functions inside of objects are known as methods.

^fe352a

now look back at `blur.py`, what it does?
1. inside of the `Image` object, there is a function called `open()`, and we pass in the name of the file to open it. then we store the output to a variable `before`
2. `before` is a struct, or technically an object. inside of which is a function called `filter()`, which takes an argument, that happens to be a function comes from the `ImageFilter` object
3. `after.save()` does, what we expect, save the file
	* we don't need `fopen()`, `fwrite()` and all the crazy codes, instead we just need `.save()`

to just run the code, enter in terminal
`python blur.py`

also as expected, not only the [[Bitmaps#^8a5fec|blur filter]] is in the `Image` and `ImageFilter` classes, it also comes with the [[Bitmaps#^d8b734|edges filter]]
___

# Python Trade-offs

now let say we would like to have a program that can check the spellings from a text file, like the task in [this](https://cs50.harvard.edu/x/2022/psets/5/speller/)
we need a hash table to store all the words in a dictionary, so that later when the program read the text file and do the spelling check on each word, it can quickly find if that word is indeed a word or not, by searching for the same word in the hash table.

how may a Python program does this?
```python
words = dict() # it gives a dictionary, otherwise known as a hash table

# To check if a `word` in `words`
def check(word):
	if word.lower() in words:  # `lower()` turns a string to all lower case
		return True
	else:
		return False

# To load all the words from a dictionary to `words`
def load(dictionary):
	file = open(dictionary, "r")  # quite similar to C's fopen()
	for line in file:             # iterate every line in a file
		word = line.rstrip()      # it strip off the right end = '\n' of a string
		words.add(word)           # add `word` to `words`
	file.close()                  # close the file, like fclose()
	return True

# To get the size of the dictionary
def size():
	return len(words)

# To unload the dictionary
def unload():
	return True
	# malloc() and free() are gone from the world of Python
	# memory are managed automatically for us
```

remember how much more code that we have to write to make a hash table, and to have all those arrays get the linked lists chained?
well, here in Python it's just `dict()` is enough to get all that works done
* all of those are still happening, it's just someone else already wrote all that stuffs and we can now use it by way of a dictionary
* however, actually we can't use `words.add()` on a `dict()`, so we need to change `dict()` to `set()`, recall, which will handle duplicates and allow us to just throw things into it by `add()`


now let us actually run the code, and yes the code will run so correctly
but, if we run it side-by-side to a C program that have the same functionality
it is very likely that a C program would be much faster than a Python program
although implementing those functinality in C is likely not an easy and quick process

so which is the better? C or Python?
it really just depends on the **workload** of the computer

if we have a very large data set, we would want to sqeeze every bit of performance out of the computer, then we might want to use a faster, a lower level language
or the program we wrote would be run on a rather small, low performance, low memory device. it's would be better to use C too.
although it might take hours, days or weeks. the process is going to be painful. it's easier to have bugs and memory leaks.

if we have a big data set, but not big enough for us to spend hours or days to write a program in C, it maybe a good idea to prioritize the human time instead
since the main point of using some modern languages is to use abstraction that other people have created for us, something like the `set()` or `dict()` that gives us a dictionary or hash table, so we don't need to reinvent the wheels


sometimes though, between the performance in C and Python, may not be that big of a gap.
there are ways to improve Python's performance, just generally it's not going to be as good as C

among those, one way to improve interpret time is to *cache* the results of that process, so that the second time, the third time the process would be notably faster

also the Python interpreter itself is actually implemented in C
so at most the interpreter could be as fast as C's compiler


so, **why** use Python?
1. we don't need to compile the code all the time
	* it's half as many steps for us the human, and it is a nice thing to optimize for
2. we may want all the fancy features that come with the language
	* if these features are useful in our cases, wouldn't it be better than shying away from them and just stick with a less suitable language?

in fact no one really use C any more.
* people would use Python or JavaScript to actually get real work done
* it's not C is bad, but just for most cases languages like Python is a better fit
___

# `hello.py`

```c
#include <stdio.h>

int main(void)
{
	printf("hello, world\n");
}
```

```python
print("hello, world")
```


`hello.c` - hello, me

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	string answer = get_string("What's your name? ");
	printf("hello, %s\n", answer);
}
```

```python
from cs50 import get_string

answer = get_string("What's your name? ")
# or print("hello, " + answer)
print(f"hello, {answer}")  # people argue this is the better style
```

also notice that the `main()` function is gone from Python
in Python we can just jump right in, the program can still run without any problem

in addition we can replace `get_string()` with `input()`
```python
answer = input("What's your name? ")
print(f"hello, {answer}")
```
they work the same
CS50 implements `get_string()` just for consistence...
___

# namespace

now let's us look back at `calculator.c`
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	int x = get_int("x: ");
	int y = get_int("y: ");
	printf("%i\n", x + y);
}
```

here is how we would do the same using Python
```python
from cs50 import get_int

x = get_int("x: ")
y = get_int("y: ")
print(x + y)
```

how about we import the whole `cs50` library, instead of just the `get_int` function
```python
import cs50

x = get_int("x: ")
y = get_int("y: ")
print(x + y)
```

it will give a *traceback*[^1], why is that?
```bash
Traceback (most recent call last):
  File "/workspaces/20377622/calculator.py", line 3, in <module>
    x = get_int("x: ")
NameError: name 'get_int' is not defined
```
[^1]: it traces back, from the earliest call to the most recent call, all of the functions that got executed. in C we would call this a *stack trace*.

Firstly think back to C, what if we use the `get_int()` from `<cs50.h>` library, yet we use another function with the same name `get_int()` from another library?
We simply can't do this. The compiler would not know how to link the 2 libraries and the functions correctly.

Python, or language like JavaScript or Java, allows us to *namespace* our functions that come from libraries. We can isolate variables and function names to their own namespace.
It's like they have their own container in memory.

so if we import all of `cs50`, we have to say the `get_int` we use is the one inside the `cs50` library
```python
import cs50

x = cs50.get_int("x: ")
y = cs50.get_int("y: ")
print(x + y)
```
much like the `Image` object in [[#^d898ac|image blurring example]]
except this time `cs50` is an namespace

is importing the whole library better?
if we're going to use most of the functions from a library, just import the whole thing
if we're going to use a few, it's better to import functions line by line
___

# typecast

how about we tweak the code a bit, removing the library and use `input()` instead?
```python
x = input("x: ")
y = input("y: ")
print(x + y)

# input
x: 1
y: 2
# output
12
```

Python interprets `x` and `y` as strings, so the `+` operator rather than sum the 2 values mathematically, it concatenates the 2 strings
the result `12` of course is yet another string, not a number

in C, there is a *ASCII to integer* function which do allow us to convert string to number
in Python, we do this by *typecasting*
```python
x = int(input("x: "))
y = int(input("y: "))
print(x + y)
```

notice it isn't `(int)(intput("x: "))`
instead we are using `int` as a function
indeed in Python, `int`, `float` are functions, which we can pass values into to do convert the type

but then, if the user actually type some words instead of numbers as `x` or `y`
the program gives us a traceback again
```bash
x: cat
Traceback (most recent call last):
  File "...", line 1, in <module>
    x = int(input("x: "))
ValueError: invalid literal for int() with base 10: 'cat'
```

this error is similar to [[CS50x 4-1 Memory#^b94d99|Segmentation Fault]] in C, but it doesn't necessarily relate to memory
this time it relates to actual runtime values not being as expected
___

# exceptions

to handle the errors like above, we would have to somehow *catch* this error
by using *exceptions*, which is a feature that C doesn't have

an exceptions, just like the `NameError` or `ValueError`, are things that can go wrong when the Python code is running
and the misbehaviors are not necessarily going to be detected until we run the code

modern languages like Python, JavaScript, Java, allow us to *try* doing something, until *except* if something goes wrong

so let us apply *exception* in the above code
```python
try:
	x = int(input("x: "))
except:
	print("That is not an int!")
	exit()

try:
	y = int(input("y: "))
except:
	print("That is not an int!")
	exit()

print(x + y)
```

now you may guess what the Python version of `cs50` library doing.
it has these `try` and `except` logic in its functions, because otherwise a rather simple program could have very long code

also a better use of `except` is to print back the error message to the user, and therefore `except` actually has parameters, like so, `except ValueError:`
then we can do something like `print(ValueError)`
___

# Truncation & Imprecision in Python

how about calculating division, since it once gave us a headche in C?
```python
from cs50 import get_int

x = get_int("x: ")
y = get_int("y: ")

z = x / y
print(z)

# input
x: 1
y: 10
# output
0.1
```

such cooperate!
remember what did we get in C when we try to do this?
result would be `0`, because dividing an integer by an integer, would give us back an integer in C. and the integer part of `0.1` is indeed `0`, this is known as [[CS50x 1-3 Data types limitation & Programming mindsets#Truncation type convertion|truncation]]

fortunately, it's not a problem any more in Python, at least in the case of division
unfortunately though, sometimes the old behavior would be useful, is it gone?
no. use `//` instead of `/` in this case. use `z = x // y`, then `z` will be `0`. ^b1d172


how about [[CS50x 1-3 Data types limitation & Programming mindsets#bits limit overflow|floating point imprecision]]?
let us do `print(f"{z:.50f}")`, it let us print 50 significant digits after the decimal point
the result is `0.10000000000000000555111512312578270211815834045410`
so it's still a thing in Python
___

# str

take a look at this `agree.c`
```c
int main(void)
{
	char c = get_char("Do you agree? ");
	
	if (c == 'Y' || c == 'y')
	{
		printf("Agreed.\n");
	}
	else if (c == 'N' || c == 'n')
	{
		printf("Not agreed.\n");
	}
}
```

to implement the same code in Python
first remember that in Python there is no char, only [[CS50x 6-1 Python Beginner Tour#data types|str]]
so the equivalent would be:
```python
s = get_string("Do you agree? ")

if s == "Y" or s == "y":
	print("Agreed.")
elif s == "N" or s == "n":
	print("Not agreed.")
```
also notice that we don't need the or operator `||`, instead we use the English word `or` here

but it would be quite not user friendly if the user must type exactly `"Y"` or `"y"` for yes
how do we check if `s` is any of `"Y"`, or `"y"`, or `"Yes"`, or `"yes"`, etc.?
well, we can make use of a list
```python
if s in ["Y", "y", "yes", "YES", "Yes"]:
	print("Agreed.")
elif s in ["N", "n", "no", "NO", "No"]:
	print ("Not agreed.")
```

but it gets a little annoying though because we would have many combinations of `yes` and `no` that we need to consider and put them in the list
how about making all the letters in `s` to lower case?
```python
if s.lower() in ["y", "yes"]:
	print("Agreed.")
elif s.lower() in ["n", "no"]:
	print("Not Agreed.")
```

`lower()`, which makes a str becomes all lower case, is a function built into the str data type
recall that this feature is made possible by Python's [[#^fe352a|OOP]]

and actually we don't really need to call `s.lower()` twice
```python
s = get_string("Do you agree? ")
s = s.lower()
```

or we can even chain functions together, which is not something we get to do in C
```python
# Since get_string() returns a str, and str has the function lower() in them
s = get_string("Do you agree? ").lower()
```

in addition, if we want to iterate through each character in a str
we can use
```python
for c in s:
	some code
```

finally, is str no longer an array of char in Python?
no. since Python don't have pointers and char anymore, string in C is very different from str in Python.
1. str has functions built-in to them because it is an *object*
2. str is *immutable*, which means a str cannot be changed
	* unlike in C we can go into a string and change its individual characters
	* in Python we can't change the original str itself, it instead returns a copy of it that makes the changes
	* so under the hood it's taking more memory than changing a string in C

it's annoying, sometimes, but protective
as we can't screw up memory and pointers, Python the language handles all of those pains and powers for us, the programmers
___

# functions in Python

now we have a program that should run a loop and call a `meow()` function 3 times
```python
for i in range(3):
	meow()

def meow():
	print("meow")
```

actually this would produce a trace back
```bash
Traceback (...):
  File ...
    meow()
NameError: name 'meow' is not defined
```

much like C, Python reads from top to bottom
by when it reads the `meow()` function, it hasn't see the keyword `meow` so it just panic

we could place the function `meow()` above the actually code
but it would not be a good practice since it is hard to find where does this program begin if we do that

what we can do is, reintroduce `main()` in Python, and place the actually code in it
```python
def main():
	for i in range(3):
		meow()

def meow():
	print("meow")

# We have to call our own main function in Python
# Unlike in C main() just get called as long as it exists
main()
```
moreover we may use something like this to call `main()` if there are some library problems
```python
if __name__ == "__main__":
	main()
```

how about if we want `meow()` to take an argument?
```python
def main():
	meow(3)

def meow(n):
	for i in range(n):
		print("meow")

main()
```

remember functions in Python
1. don't bother specifying return types of the functions
2. don't bother specifying the type of the arguments or variables

finally, Python too has [[C - 7 Variable Scope|local and global variables]]
___

# Others

Python doesn't have *constant*
the mentality in the Python community is
> [!quote]
> If you don't want some value to change, don't touch it.

Python doesn't have [[CS50x 1-3 Data types limitation & Programming mindsets#prompting input with do-while loop|do-while loop]] too
the *Pythonic* way of doing so is:
```python
while True:
	n = get_int("Height: ")
	if n > 0:
		break
```

and surely we can encapsulate this in a function
```python
def get_height():
	while True:
		n = get_int("Height: ")
		if n > 0:
			return n
# or
def get_height():
	while True:
		n = get_int("Height: ")
		if n > 0:
			break
	return n
```

but it seems the second version is a bit wrong, where?
in that version `n` would exist only in the while loop right? how come we can return `n` outside of the while loop?
well, if it's in C, yes it would be a bug if we do so
but in Python, once we create a variable in Python, it's available everywhere within that function, even outside of the loop that we defined the variable

also we can improve the code a bit by protecting the program when the user enters something invalid and then prompt the user again, using [[#exceptions]]
```python
def get_height:
	while True:
		try:
			n = int(input("Height: "))
			if n > 0:
				break
		except ValueError:
			print("That's not an integer!")
	return n
```
___
