# named argument

`print()` automatically create new line,
what to do if we don't want that?

we need to specific what the line ending should be, as another argument to `print()`
however it requires more than simply doing `print("?", "")`

arugments can have 2 types in Python
1. *positional* argument
	* it means the arguments are comma separated
	* this is what we did all the time in C
2. *named* argument
	* in Python people can give name to the function's arguments
	* `print()` in Python has a named argument, which is `end`, that specific the str to add to as the line ending
	* it doesn't matter what order they are in, so it's helpful especially when a function takes 10 or 20 arguments

so if we need to print 4 `?` in a row, `????`
```python
for i in range(4):
	# If we read the document, `end` by default is "\n"
	print("?", end="")
# We print nothing here to move the prompt to a new line
print()
```
___

# `*` for str

cool, we solved our problem
but actually we don't even need it to be a 3 line code

we can just do
```python
print("?" * 4)
```
and the outcome is just the same

`*` can apply on str now in Python
it means concatenate the str on the left, 4 times over
___

# List but not array

remember `score.c`, where we learn about [[CS50x 2-3 Array#Array|array]]?
now let us create a `score.py`
```python
scores = [72, 73, 33]

average = sum(scores) / len(scores)
print(f"Average: {average}")
```

but surely we won't hard code the scores
```python
scores = []
# again, this is not something we can do in C
# because here we would be declaring an empty array in C
# in Python though this is a list, which is lengthless and can grow and shrink
# yet again, the pointers or memory stuffs are handled all by Python already

for i in range(3):
	score = get_int("Score: ")
	scores.append(score)
	# append() is a method in the list object

average = sum(scores) / len(scores)
print(f"Average: {average}")
```

but if we don't like using `append()`
we can do `scores += [score]` or `scores = scores + [score]`
* it concatenates 2 lists together
* the square brackets `[score]` are necessary because we're concatenating a list to a list, not a int to a list
___

# Command Line Argument in Python

[[CS50x 2-5 main()#main|argument vector]] is not built-in any more
they are factored out in the `sys` library
if we want to use it we have to import it back first
```python
from sys import argv

if len(argv) == 2:
	print(f"hello, {argv[1]}")
else:
	print("hello, world")
```

later when we run the program we type
`python argv.py David`
* notice `python` is not a part of the argument vectors
* `argv.py` is stored in `argv[0]`, and `David` is stored in `argv[1]`

we can even iterate through each argument vectors
```python
from sys import argv

for arg in argv:
	print(arg)
```
___

# Slicing

but what if we don't want to also print `argv.py`?
```python
for arg in argv[1:]:
	print(arg)
```

the `argv[1:]` part is known as *slicing* a list
* it makes the loop starts from position 1, going all the way to the end
* if we want to slice the end of the list, similarly we use `argv[:-1]`
___

# exit status

similar to the case in command line argument, [[CS50x 2-5 main()#exit status|exit status]] is moved to the `sys` library
just like C, we typically use the exit code `0` to signify success and `1` to signify error

```python
from sys import argv, exit

if len(argv) != 2:
	print("Missing command-line argument")
	exit(1)

print(f"hello, {argv[1]}")
exit(0)
```
___
