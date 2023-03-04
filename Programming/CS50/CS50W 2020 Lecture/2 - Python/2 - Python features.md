# Object-oriented programming

it's a programming paradigm, so to speak
* a way of thinking about the way that we write programs

In object-oriented programming, we think about the world in terms of objects
* Objects might store information
* and might support some operations, some methods or functions
	* that can operate on those objects
___

## Creating a type
create a class, of objects, to represent something
* think about classes as templates for a type of object

eg. a point, which has a x and a y
```python
class Point():
	def __init__(self, x, y):
		self.x = x
		self.y = y

# Create a new Point object
p = Point(2, 8)
print(p.x)
print(p.y)
```

`def __init__(self, x, y):`
* it's a magic method that will automatically be called every time we create an object of the class
* all functions that operate on objects themselves, otherwise known as methods, take the first argument called `self`
	* it references the object that it is dealing with
___

## eg. Flight

let's try to make a program that register flights and passengers of a flight
```python
class Flight():
	def __init__(self, capacity):
		self.capacity = capacity
		self.passengers = []
	
	def add_passenger(self, name):
		if not self.open_seat():  # it's equivalent to `if self.open_seat() == 0`
			return False
		self.passengers.append(name)
		return True
	
	def open_seats(self):
		return self.capacity - len(self.passengers)

flight = Flight(3)  # only 3 people can go on this flight, but no more

people = ["Harry", "Ron", "Hermione", "Ginny"]
for person in people:
	success = flight.add_passenger(person)
	if success:  # we can optimize a bit by removing the `success` variable
		print(f"Added {person} to flight successfully.")
	else:
		print(f"No available seats for {person}.")
```
___

# Decorators

similar to how we can modify a number or any other value in Python,
decorators are a way in Python of taking a function and modifying it
* adding some additional behavior to that function
* it takes a function as input, and returns a modified version of that function as output

in some other programming languages, functions just exist in their own
* they can't be passed in or out as input or output
in Python, a function is just a value like any other
* they can be passed in or out as input and output, as if it was any other data type

> [!info]
> This is know as a *functional programming paradigm*.
> It basically means, functions are values.

eg. let's just make a decorator that will do something before and after running the function
* inside of which is a function, in which we will define the behaviors that we want
* since this kind of functions usually wrap up the input function, it's called a wrapper function
```python
# this is the decorator
def announce(f):
	# this is a wrapper function
	def wrapper():
		print("About to run the function...")
		f()
		print("Done with the function.")
	return wrapper

@announce
def hello():
	print("Hello, world!")

hello()
```

eg. imagine in a web application, we want some certain functions to be able to run, only if a user is logged in
* we can write a decorator that check if the user is logged in
* then put on the decorator to all the log-in required functions
___

# Lambda

```python
people = [
	{"name": "Harry", "house": "Gryffindor"},
	{"name": "Cho", "house": "Ravenclaw"},
	{"name": "Draco", "house": "Slytherin"}
]

people.sort()
print(people)
```

If we run this program, it'd give us an error
```bash
Traceback (...):
	File ...
		people.sort()
TypeError: '<' not supported between instances of 'dict' and 'dict'
```

which is interesting because we didn't use any `'<'` symbol at all
* actually `people.sort()` used `'<'`

the problem here, for sure, is that Python doesn't know how to sort dictionaries
* we need to tell that

```python
people = [
	{"name": "Harry", "house": "Gryffindor"},
	{"name": "Cho", "house": "Ravenclaw"},
	{"name": "Draco", "house": "Slytherin"}
]

def f(person):
	return person["name"]

people.sort(key=f)
print(people)
```

surely, we can use lambda expression
```python
...
people.sort(key=lambda person: person["name"])
print(people)
```
* `lambda person: person["name"]` is actually a complete function already
___

# try-except expression

```python
x = int(input("x: "))
y = int(input("y: "))

result = x / y
print(f"{x} / {y} = {result}")
```

then if we input `y` as `0`, we can get an error
```bash
...
ZeroDivisionError: division by zero
```

when exceptions happen, we don't necessarily have to look at this messy error messages
* because we probably don't want the users looking at this messy stuffs
* because we don't want the program to look crashed, although it is
```python
import sys

x = int(input("x: "))
y = int(input("y: "))

try:
	result = x / y
except ZeroDivisionError:
	print("Error: Cannot divide by 0.")
	sys.exit(1)  # exit the program

print(f"{x} / {y} = {result}")
```

similarly, exception happens when we input `x` or `y` as `hello`
* since `int()` can't parse string to int
* but we can handle it the same way
```python
import sys

try:
	x = int(input("x: "))
	y = int(input("y: "))
except ValueError:
	print("Error: Invalid input.")
	sys.exit(1)

try:
	result = x / y
except ZeroDivisionError:
	print("Error: Cannot divide by 0.")
	sys.exit(1)

print(f"{x} / {y} = {result}")
```
___
