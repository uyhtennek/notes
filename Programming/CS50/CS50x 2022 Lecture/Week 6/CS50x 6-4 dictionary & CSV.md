# Linear Search in Python
just some example to showcase how easy it's to do searching in Python

search for a number
```python
import sys

numbers = [4, 6, 8, 2, 7, 5, 0]

if 0 in numbers:
	print("Found")
	sys.exit(0)

print("Not found")
sys.exit(1)
```

similarly search for a string
```python
import sys

names = ["Bill", "Charlie", "Fred", "Georage", "Ginny", "Percy", "Ron"]

if "Ron" in names:
	print("Found")
	sys.exit(0)

print("Not found")
sys.exit(1)
```
___

# dictionary

remember how we can [[CS50x 3-3 Data Structure#Custom Data Structure|couple data types together]] in C?
Python also has objects, or generally classes, that allow us to do the same.
but yet it also has another general purpose data type named *dictionary*, which associate keys with values
```python
from cs50 import get_string

people = {
	"Carter": "+1-617-495-1000",
	"David": "+1-949-468-2750"
}

# If we want an empty dictionary, we can do alternatively
people = dict()
# or
people = {}
```

by the way,
the colon `:` is a super common way in programming world to associate keys with values
and they are a very common concept not only in Python, but also CSS and HTML and others

and then if we want to find a person's number
```python
name = get_string("Name: ")
if name in people:
	print(f"Number: {people[name]}")
```
notice just like how we can index into a list using square brackets `[]` and a number
we can conveniently index into a dictionary using sqaure brackets `[]` and a key

in C or in an array, an number in `[]` would mean go to that location
but that is not the case in dictionary, it's like a table with 2 columns, where we lookup 1 column it gives back the another column
___

# CSV file
* *CSV* stands for *comma separated values*, so CSV file is a file with comma separated values in it
* most spreadsheet softwares like Google Sheet, Microsoft Excel, allow exporting data in CSV format
* in addition one reason why Python is so popular for data science and analytics
	* other than it being such a high-level language and so beginner-friendly
	* it's actually really easy to manipulate data and run analytics in Python

Python comes with a powerful `csv` library
and here is how we can store or write some values in a CSV file
```python
import csv
from cs50 import get_string

file = open("phonebook.csv", "a")

name = get_string("Name: ")
number = get_string("Number: ")

writer = csv.write(file)
writer.writerow([name, number])

file.close()
```

but actually, Python is so thoughtful that it afraids people would not `close()` the opened file, it has a special `with` logic, that simply handle the `close()` for us
```python
name = get_string("Name: ")
number = get_string("Number: ")

with open("phonebook.csv", "a") as file:
	writer = csv.write(file)
	writer.writerow([name, number])
```

how about reading from a CSV file?
here let say we have a quesionaire where we ask a bunch of people which house in Hogwarts, you know, in Harry Potter world, they want to be in
```python
import csv

houses = {
	"Gryffindor": 0,
	"Hufflepuff": 0,
	"Ravenclaw": 0,
	"Slytherin": 0
}

with open("hogwarts.csv", "r") as file:
	reader = csv.reader(file)
	next(reader) # This ignore the first line of the file
	for row in reader:
		# row is a list, reader store in row the values in each line of the CSV
		# given which column the value in
		# in this case assume `row[1]` is the people's answer
		house = row[1]
		houses[house] += 1

# Print the result
for house in houses:
	count = houses[house]
	print(f"{house}: {count}")
```
`reader` is an object in Python that can loop over to read the file one row at at time

however, there is another way for doing the read part
```python
with open("hogwarts.csv", "r") as file:
	reader = csv.DictReader(file)
	for row in reader:
		house = row["House"]
		houses[house] += 1
```
`DictReader()` still returns every row from the file, but it doesn't store them in a [[CS50x 6-3 more Python rules#List but not array|list]] for each row like `reader()` does, it stores them in a [[#dictionary]] for each row
* and the keys are what's in the first line of the file
	* also they are named *fields*
* using `DictReader()` is arguably a better design
	* because it's more robust against changes, to use dictionary instead of list
	* simply dragging and dropping the column in Google Sheet would break a program that uses `reader()`, but a program that uses `DictReader()`
___
