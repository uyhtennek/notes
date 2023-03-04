# Data collection as CSV
eg. [Google Form](https://www.google.com/intl/zh-TW_hk/forms/about/)

Google Form has a nice feature that can export the collected data to [Google Sheet](https://www.google.com/sheets/about/)
from there we can then export the collected data as [[CS50x 6-4 dictionary & CSV#CSV file|CSV file]]

but why we prefer CSV file? why not something like a Excel file?
* it's a *flat-file database*
	* it means the file is just a simple text file
		* ASCII or Unicode text
	* but still the text follows a structure
		* for CSV the structure is
		* column seperated by comma `','`;
		* row seperated by an invisible new line `'\n'` character
* it's completely *portable*
	* we can use the same CSV file in any operating system
		* Mac / Windows / Linus / Android / iOS / etc.
	* doesn't require any software
		* Excel file can only opened with Microsoft Excel, and people would have to buy Microsoft Excel
		* if it's a .number file, it can only be used in a Mac but not PC

however, what happen when the data itself have comma `','` in it?
* usually that piece of data will be encapsulated with a double quote `""`
	* to *escape* from being interpreted as a CSV comma `,`
```csv
Title, genres
New Girl, Comedy
Friends, Comedy
Friday Night Lights, "Drama, Family, Sport"
The Office, Comedy
Modern Family, "Comedy, Family"
Game of Thrones, "Action, Adventure, Drama, Fantasy, Thriller, War"
Breaking Bad, "Crime, Drama"
```
* Google, Microsoft, Apple uses this solution
___

# Cleaning data

it's noteworthy that data can get messy fast
* `'Breaking Bad'`, `'breaking bad'`, `'Breaking bad'` means the same TV shows
* but they can be interpreted as different TV shows if our program is not forgiving
* not to mention there could be user errors like typo

1. how to remove duplicates?
```python
import csv

titles = []

with open("favorites.csv", "r") as file:
	reader = csv.DictReader(file)
	for row in reader:
		it not row['title'] in titles:
			titles.append(row['title'])

for title in titles:
	print(title)
```

2. remove whitespaces and near duplicates, like the above `'breaking bad'` example
```python
titles = []

with open("favorites.csv", "r") as file:
	reader = csv.DictReader(file)
	for row in reader:
		# `strip()` can remove whitespaces from the start and the end of a string
		# `upper()` turns a string to all upper case
		title = row['title'].strip().upper()
		it not title in titles:
			titles.append(title)

for title in titles:
	print(title)
```

or a better way to do this would be using a [[CS50x 6-1 Python Beginner Tour#^b87f09|set]] instead of a [[Python Syntax#~~Arrays~~ Lists|list]]
```python
titles = set()

with open("favorites.csv", "r") as file:
	reader = csv.DictReader(file)
	for row in reader:
		title = row['title'].strip().upper()
		titles.add(title)

for title in titles:
	print(title)
```

3. sort the data
```python
for title in sorted(titles):
	print(title)
```
so that it's easier for us to see all the variances that we did not fix by focusing on whitespaces and capitalization alone
___

# Counting data

but we have throw off how many times a data is inputted by different users
how do we keep that?

```python
titles = {}
# or titles = dict()

with open("favorites.csv", "r") as file:
	reader = csv.DictReader(file)
	for row in reader:
		title = row['title'].strip().upper()
		titles[title] += 1

for title in sorted(titles):
	print(title)
```
this is a reasonable first attempt, but it'd give a `KeyError`
* because when the program runs the line `titles[title] += 1`, `titles[title]` doesn't exist yet
	* the key doesn't exist yet
* so the program doesn't know where it should do the `+= 1`

1. to fix it
```python
...
	for row in reader:
		title = row['title'].strip().upper()
		if title in titles:
			titles[title] += 1
		else:
			titles[title] = 1
...
```
or
```python
...
	for row in reader:
		title = row['title'].strip().upper()
		if not title in titles:
			titles[title] = 0
		titles[title] += 1
...
```

2. also we would want to print the counts
```python
for title in titles:
	print(title, titles[title])
```
___

# Sorting a dictionary

next thing, again, we would like to sort the data
```python
for title in sorted(titles):
	print(title, titles[title])
```
* by default, when sorting a dictionary with the `sorted()` function, it's sorted by keys, not by values
* but we can manipulate the behavior of the `sorted()` function a bit, by putting in arguments
	* `sorted(titles, reverse=True)`
	   sort the titles, in reverse order

to sort by values, we actually need a helper function
```python
...
def get_value(title):
	return titles[title]

for title in sorted(titles, key=get_value, reverse=True):
	print(title, titles[title])
```
* if we don't want to sort by the keys, we need to introduce a parameter `key`
* the type of that parameter is actually a function name
	* notice we input a function name `get_value`, instead of a function call `get_value()`
	* and that function will determine what to sort by
		* the function returns the thing to sort by
		* in this case it's the value of the dictionary `titles`
___

# Lambda function

however, it feels like the `get_value()` function is pretty redundant
* we're not going to reuse it in multiple places. we just use it once and then throw it away.
* we don't really need to keep it around

A lambda function is much like that
* it's a function with no name, an *anonymous function*
* since the function is just gonna be used one time and one place only, why bother giving it a name at all?

how to use it?
```python
for title in sorted(titles, key=lambda title: titles[title], reverse=True):
	print(title, titles[title])
```

one thing about Lambda function in Python is that
* it supports one line only
	* if the logic needs multiple lines, just use a normal function and give it a name instead
* some other programming languages though, can have a lambda function with multiple lines
	* like JavaScript
___

# Futher Cleaning Data

now say if we just want to know how many people like the TV show, The Office
```python
counter = 0

with open("favorites.csv", "r") as file:
	reader = csv.DictReader(file)
	for row in reader:
		title = row['title'].strip().upper()
		if title == "THE OFFICE":
			counter += 1

print(f"Number of people who like The Office: {counter}")
```

but people may type either `The Office` or `Office`, but they mean the same TV show
how to fix this?

simple.
```python
...
		if title == "THE OFFICE" or title == "OFFICE":
			counter += 1
...
```

but we can see that is not quite scalable
* people can type `The Office`, `Office`, `Ze Office`, `it's the office`, etc.
* meaning the same TV show

how about doing this?
```python
...
		if "OFFICE" in title:
			counter += 1
...
```

it is better
but something like `Office Hours` get counted as well although it is totally a different TV show

from this point we may want to just start using [[CS50x 7-2 Regular expression#Regular expression|regular expression]]
___
