# semi-colon
Python statements needn't end with semicolons `;` !
We can include them but it's not necessary.
___

# Variables

* No type specifier
* Declared by initialization only

```python
# Correct
x = 54
# Not gonna work
x
```

* string value can be encapsulated by either single quote `''` or double quote `""`
	* it's useful when we have single quote `''` or double quote `""` in our string values
```python
phrase = "This is 'CS50'!"
phrase = 'This is "CS50"!'
```
___

# Conditionals

* we use the word `or` instead of double vertical bar `||` like we did in C
* same goes to `and`, and `not`
* Python doesn't have `else if`, but have `elif` as a substitute

```python
if y < 43 or z == 15:
	# code goes here
```

```python
if y < 43 and z == 15:
	# code block 1
else:
	# code block 2
```

```python
if coursenum == 50:
	# code block 1
elif not coursenum == 51:
	# code block 2
```

[[C - 4 conditional statements#Ternary operator|Ternary operator]] looks quite different than C though
```c
char var = get_char();
bool alphabetic = isalpha(var) ? true : false;
```
```python
letters_only = True if input().isalpha() else False
# input() is a function native to Python
# we can use that to collect user input at the command line
```

also notice that `true` and `false` in Python are capitalized
* `True` and `False`
___

# Comments
Python uses the pound sign `#` to start a comment
```python
# this is a comment in python
```
___

# Loops

Python only has 2 kinds of loop
* while
* for

```python
counter = 0
while counter < 100:
	print(counter)
	counter += 1
```

```python
for x in range(100):
	print(x)
```

* notice that we don't have `++` or `--` in Python
   we need to very explicitly call out `counter += 1` / `counter -= 1`

* also notice that we don't need the backslash n `\n` in the `print()` function to tell the program to get to the next line
	* by default Python adds the `\n` as a [[CS50x 6-3 more Python rules#named argument|line ending]] in the `print()` function

* `range()` is a function that will give us a list, from 0 to 99 in this case
	* how about we want to count like `0, 2, 4, 6, 8, ..., 98` ?

```python
for x in range(0, 100, 2):
	print(x)
```
___

# ~~Arrays~~ Lists
Here is where things start to get better than C.

* Python arrays are actually **lists**
	* not fixed in size, can grow or shrink as needed
	* not have to be the same type
		* we can have a list like `50, "CS50", False, var, ...`
	* can always add more things on, splice or remove things from the middle of the list

* Declaration
```python
# declare an empty list
nums = []
nums = list() # `list()` is a function to create a list

# declare an int list
nums = [1, 2, 3, 4]
nums = [x for x in range(500)] # Python supports *list comprehension*
# it's using a for loop to generate a list of numbers, 0 - 499 in this case
```

* Tacking things on
```python
nums.append(5)         # add 5 to the end of list
nums.insert(4, 5)      # insert in the 4th position, the value 5
nums[len(nums):] = [5] # create another list [5], put it after position len(nums)
```
ps. `len()` calculate the length of any arbitrary list
___

# Tuples
an ordered, immutable set of data

* not quite like anything compararable to C
* great for associating collections of data
	* sort of like a struct in C
	* except the value would never change
* really fast to navigate

```python
# a list of tuples
presidents = [
	("George Washington", 1789),
	("John Adams", 1797),
	("Thomas Jefferson", 1801),
	("James Madison", 1809)
]
```

```python
# iterate a list of tuples
for prez, year in presidents:
	print("In {1}, {0} took office".format(prez, year))
	# {1} will get the value of `year` and {0} will get the value of `prez`
	# in alternative we can also do
	print("In {}, {} took office".format(year, prez))
```
___

# Dictionaries
close to a [[CS50x 5-4 Hash Table|hash table]]

* hash tables were not native to C
	* but are native in many programming languages
* dictionaries allow us to specify list indices with *keys* instead of *integers*

```python
# a dictionary
pizzas = {
	"cheese": 9,
	"pepperoni": 10,
	"vegetable": 11,
	"buffalo chicken": 12
}
# the format is `<key>: <value>`
```

```python
# change an item's value
pizzas["cheese"] = 8

# access an item's value
if pizza["vegetables"] < 12:
	# do something

# add an item / add a key-value pair
pizzas["bacon"] = 14
```

how do we iterate through a dictionary?
```python
for pie in pizzas:
	# use pie in here as a stand-in for "i"
	print(pie)

# result
cheese
vegetable
buffalo chicken
pepperoni


# but how about iterating through the values instead
for pie, price in pizzas.items():  #`items()` transform the dictionary into a list
	print(price)

# result
12
10
9
11

# also here is how to iterate through both keys and values
for pie, price in pizzas.items():
	print("A whole {} pizza costs ${}".format(pie, price))

# result
A whole buffalo chicken pizza costs $12
A whole cheese pizza costs $9
A whole vegetable pizza costs $11
A whole pepperoni pizza costs $10
```

notice that the results may not be in order
same goes to the list that we transform `pizzas` into using `items()`
___

# String interpolation

these all print the same sentense
```python
1. print("A whole {} pizza costs ${}".format(pie, price))
2. print("A whole " + pie + " pizza costs $" + str(price))
3. print("A whole %s pizza costs $%2d" % (pie, price)) # avoid this; deprecated
```
___

# Functions

* don't need to specify
	* return type of the function
	* data types of any parameters
* because data type doesn't matter in Python
* still need to specify the name of the function and any parameters though

* we introduce a function with the keyword `def`
	* stands for *define*
```python
# define a function
def sqaure(x):
	return x * x
	# or
	return x ** 2  # `**` is an exponentiation operator
```

* Python doesn't need a `main` function
	* the interpreter just reads and runs from top to bottom
	* we have to explicitly force it if we want it
```python
# force a main() function
if __name__ == "__main__":
	main()
```
* almost always placed at the very end of the Python file
___

# Objects
* Python is an [[CS50x 6-2 C and Python#^fe352a|object-oriented programming language]]
* cloest thing in C is structure

* C structures contain a number of *fields* / *properties*
	* particularly in an object oriented context, we call them *properties*
	* properties themselves cannot ever stand on their own
```c
struct car
{
	int year;
	char *model;
}

struct car herbie;
# this is invalid. since properties cannot stand on their own.
year = 1963;
model = "Beetle";
```

* Objects, however, have *properties* and *methods*
	* *method* is a function that is an inherent to the object
* like properties, methods cannot stand on their own
	* they mean nothing outside of the object
	* we can't call that function just out of the blue on anything, but only can call the function on objects of the type where the function is defined
* since properties and methods depend on objects, objects are very important
	* that's how the term *object-oriented* comes from

```python
# call a method
obj.method()
# not `method(obj)`
```

To define an object
* we define a type of object using the `class` keyword
* classes require an initialization function
	* generally known as a *constructor*
	* which sets / assigns the starting values of some or all properties of the object
* we also have to define functions or methods that can apply to the object
	* every function defined in a class has at least one parameter
	* cannonically (= in convention) named `self`
	* it's a reference to the object instance

```python
# define an object
class Student():
	
	# the constructor / initialization function
	# always called `__init__`
	def __init__(self, name, id):
		self.name = name
		self.id = id
	
	def changedID(self, id):
		self.id = id
	
	def print(self):
		print("{} - {}".format(self.name, self.id))

# here's how to use the object and methods
jane = Student("Jane", 10)
jane.print()
jane.changeID(11)
jane.print()
```
___

# Style
good style is **crucial** in Python
* in C, style only matters to the people who read the code
* in Python, style matters to the computer too

* Tabs and indentation matter
	* blocks or curly braces `{}` in C are replaced with indentation in Python
* Except declaring dictionaries still use curly braces `{}`
___

# Including files
* instead of calling *header files* or *libraries*
* in Python we generally call them *modules*

```python
# include / import the whole cs50 module
import cs50

# import just the `get_int` function from cs50
from cs50 import get_int
```
___
