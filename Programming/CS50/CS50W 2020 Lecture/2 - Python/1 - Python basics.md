# Hello, World

```python
print("Hello, world!")
```

`.py` is the extension for a python program
* eg. `hello.py`

to run a python program, we need a python interpreter
* one of them happens to be named as `python`
	* so we'd run a python program using `python <python file>`
	* eg. `python hello.py`
* also note that there's no need to compile the file first before running it
___

# Variables

```python
a = 28        # int
b = 1.5       # float
c = "Hello!"  # str
d = True      # bool
e = None      # NoneType
```

note that there's no need to specify the variable types
* Python knows what the type should be because we infered it
* also note that Python does have types, just that we don't need to explicitly state them
___

# Input

```python
name = input("Name: ")
print("Hello, " + name)
```
___

# fstrings
short for formatted strings

```python
name = input("Name: ")
print(f"Hello, {name}")
```
___

# Conditions

```python
n = input("Number: ")
if n > 0:
	print("n is positive")
elif n < 0:
	print("n is negative")
else:
	print("n is zero")
```

note that we need the indentation
* it tells the program which lines are inside of a if statement and which are not


## Exceptions

note that the above program causes an error, or an exception
```bash
Traceback (most recent call last):
	File "conditions.py", line 3, in <module>
		if n > 0:
TypeError: '>' not supported between instances of 'str' and 'int'
```

so `n` is a `str`, but why might that be?
* `input()` function always give us back a `str`
* so we need to convert, or cast, the input to `int`
	* using `int()`

so the fix is `n = int(input("Number: "))`
___

# Sequences
it means sequences of data, so things like string, list, ....

* string
```python
name = "Harry"
print(name[0])
```

* list
```Python
names = ["Harry", "Ron", "Hermione"]
print(names)
print(names[0])
```

* tuple
	* tuple is often used if we have a couple of values that aren't going to change but we would like to store them together, as one single element
		* like a pair of values together, three values together, ....
		* we can't to anything to a existing tuple
	* eg. a point should have both x and y
		* we can create two variables for it
		* but since it really is just one unit, maybe it's better to couple them
```python
coordinate = (10.0, 20.0)
```

| Data Structures | characteristics |
| :-: | --- |
| list | sequence of mutable values |
| tuple | sequence of immutable values |
| set | collection of unique values |
| dict | collection of key-value pairs |
| ... | |

we can use `len()` to see how many elements are in a sequence
___

## list

```python
# Define a list of names
names = ["Harry", "Ron", "Hermione", "Ginny"]
# print all elements
print(names)
# print first element
print(names[0])

# add a value to the end of an existing list
names.append("Draco")
# sort the list
names.sort()

print(names)
```
___

## set

```python
# Create an empty set
s = set()

# Add elements to set
s.add(1)
s.add(2)
s.add(3)
s.add(4)
print(s)  # output: {1, 2, 3, 4}
# note that the order is not guaranteed, it just happen to be in this case

# note that no element repeat in a set
s.add(3)
print(s)  # output: {1, 2, 3, 4}

# remove an element
s.remove(2)
print(s)  # output: {1, 3, 4}

print(f"The set has {len(s)} elements.")
```
___

## dictionary
it can map keys and values

the way we declare a key-value pair is by saying `<key>: <value>`
* when we look up a key, it gives us the value
```python
houses = {"Harry": "Gryffindor", "Draco": "Slytherin"}
print(houses["Harry"])

# add a key-value pair
houses["Hermione"] = "Gryffindor"
print(houses["Hermione"])
```
___

# Loops

This is the simplest loop in Python
* go through a list of elements
* for each element, name it as `i`
```python
for i in [0, 1, 2, 3, 4, 5]:
	print(i)
```

`range()` gives a list of numbers
* `range(6)` gives 0-5
```python
for i in range(6):
	print(i)
```

we can loop through any type of sequence
```python
names = ["Harry", "Ron", "Hermione"]
for name in names:
	print(name)
```
```python
name = "Harry"
for character in name:
	print(character)
```
___

# Functions

```python
def square(x):
	return x * x

for i in range(10):
	print(f"The square of {i} is {square(i)}")
```

if we call functions coming from another file, it can give us an error
```bash
...
NameError: name 'square' is not defined
```
* `NameError` is a common one
	* in this case it tells us that we never said what `'square'` is
	* it doesn't have a definition

by default, Python files don't know about each other
If we want to use a function that was defined inside of another file, we need to import it
```python
from functions import square

for i in range(10):
	print(f"The square of {i} is {square(i)}")
```

alternatively we can import the entire module
* as opposed to importing just a function from that module
```python
import functions

for i in range(10):
	print(f"The square of {i} is {functions.square(i)}")
```

there are other Python built-in modules that we can import as well
* eg. Python's CSV module, math module, .....
___
