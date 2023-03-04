> [!info] Testing
> verifying and making sure that our code are in fact, correct

Ideally, we'd like some way to be able to efficiently and effectively test our code
* as our programs grow more complicated
___

# `assert`

we start off simple
we want to test a function in Python is correct or not
* we can do that with the aid of a function, `assert`

it asserts, or states, that something should be true
* and if something is not true, `assert` would throw an exception
* so whoever is running the program or the command, knows something went wrong

now, let's start with a function that simple take a number and return the square of that
```python
def square(x):
	return x * x
```

what we can do to verify this function, surely there is the `print` function
```python
print(square(10))
```

and then if it outputs the correct answer,
we can say to ourselves, "okay, the function is indeed working"
* but here we need to do the math in our head
* which is not we would prefer because it's not automated

so then, we can do the math upfront, but make the process automated, by saying
```python
print(square(10) == 100)
```

well, that's automated, but that isn't very useful
* but now it's time for `assert` to come in to play
```python
assert square(10) == 100
```

now if we run the program, nothing would happen
* because when an `assert` statement runs, and if its expression that it's checking turns out to be true, the code simply just continue
	* so the `assert` statement is ignored
	* no output, no side effect of any sort
* this is helpful because we can assert, or verify, something at the middle of some code
	* and if everything is correct, it's as if the assert statement isn't there at all
	* then the code simply just continue
* but then what happen if there was a bug?
	* so now imagine we have a bug in our `square` function
		* maybe we accidentally typed `x + x` instead of `x * x`
	* then we would get an exception if we run the code
```bash
Traceback (most recent call last):
	File "assert.py", line 4, in <module>
		assert square(10) == 100
AssertionError
```

so we can imagine, one way to do testing, is just
by including a number of different assert statements
* so for more complex functions or code, we can ensure no matter what conditional branch the program decides to follow, the code are still correct, using a bunch of `assert`
* it's good for catching bugs as well

> [!info] Test-Driven Development
> It simply means, developing while keeping this notion of testing in mind.
> 
> One of the best practices would be, when ever we encounter some bug in a program, we'll first fix the bug, but then we'll also want to write a test, that verifies that the new behavior is working as expected.
> 
> Then as the project grows larger, we can always run those existing set of tests to make sure, no new changes that we make to the program break any existing thing.
> 
> Because, when programs get complex enough, we basically just can't test everything by hand. So we need to automate the process.

so `assert` can throw an exception when ever there is something wrong
and using Python we also can `catch` those exceptions, so that we can handle the errors and prevent a total break down
___

# Automated Test

testing the `square` function probably isn't a big deal, given that it's so simple
we now want to run tests for a more complex function
* eg. we have a function that checks if an number is prime or not

`prime.py`
```python
def is_prime(n):
	if n < 2:
		return False
	for i in range(2, n):
		if n % i == 0:
			return False
	return True
```

before implementing tests, we want to do a little optimization here
* that is, we don't need checking all the numbers between `2` and `n`
```python
import math

def is_prime(n):
	if n < 2:
		return False
	for i in range(2, int(math.sqrt(n))):
		if n % i == 0:
			return False
	return True
```

now as the function is finished, we can start testing
we can always start off testing manually
* one easy way would be go to the Python interpreter and run the function with different input and see if any thing noticeably wrong

```python
from prime import is_prime
is_prime(5)
# output True
is_prime(10)
# output False
...
```

it seems working, then maybe we should start writing automated test
* so we create a file, `test.py` for example
* instead of using `assert`, it just uses a Boolean expression
```python
from prime import is_prime

def test_prime(n, expected):
	if is_prime(n) != expected:
		print(f"ERROR on is_prime({n}), expected {expected}")
```

now we can get to the Python interpreter, and start testing again
* it's still manual, but a bit friendly than the last one
```python
from test import test_prime
test_prime(5, True)
# output nothing, so it's correct
test_prime(10, False)
# output nothing
test_prime(25, False)
# output: Error on is_prime(25), expected False
```

now you get how this kind of test function, like `test_prime`, work
* we can easily automate this testing processing
	* by letting the computer mimicks us doing the tests by hand
* bascially just run the same tests, but via scripts instead of hand
	* this script we would name it, `test.sh`
	* `.sh` for *shell* script
```shell
python3 -c "from test import test_prime; test_prime(1, False)"
python3 -c "from test import test_prime; test_prime(2, True)"
python3 -c "from test import test_prime; test_prime(8, False)"
python3 -c "from test import test_prime; test_prime(11, True)"
python3 -c "from test import test_prime; test_prime(25, False)"
python3 -c "from test import test_prime; test_prime(28, False)"
```
* now we can run the script
```bash
./test.sh

# output
ERROR on is_prime(8), expected False
ERROR on is_prime(25), expected False
```
___

## `unittest`

but ultimately, it seems like a quite painful job to do to write down all the tests for the computer
luckily we have some libraries from the job

`unittest` is one of the most popular in Python
* it's designed to very quickly write tests
* and it is built in with an automated test runner
	* that will run all of the tests for us

so we can now upgrade our `test.py`
```python
import unittest
from prime import is_prime

class Tests(unittest.TestCase):
	
	def test_1(self):
		"""Check that 1 is not prime."""
		self.assertFlase(is_prime(1))
	
	def test_2(self):
		"""Check that 2 is prime."""
		self.assertTrue(is_prime(2))
	
	def test_8(self):
		"""Check that 8 is not prime."""
		self.assertFalse(is_prime(8))
	
	...

if __name__ == "__main__":
	unittest.main()
```

`class Tests(unittest.TestCase)`
* this class will contain all of the tests
* this class is inherits from, or derives from, `unittest.TestCase`
	* which means this class going to be a class, that defines a whole bunch of functions, each of which is something that we want to test

`self.assertFalse`
`self.assertTrue`
* simple enough, just assert something as true or false

`unittest.main()`
* run all the unit tests defined

so when we run the program, we get
```bash
python3 test.py
# output
...F.F
====================
FAIL: test_25 (__main__.Tests)
Check that 25 is not prime.
--------------------
Traceback (most recent call last):
	...
AssertionError: True is not false

=====================
FAIL: test_8 (__main__.Tests)
Check that 8 is not prime.
---------------------
...

---------------------
Ran 6 tests in 0.001s

FAILED (failures=2)
```

`...F.F`
* `.` represents a test that succeeded
* `F` meaning the test is **f**ailed

`Check that 25 is not prime.`
* this sentence is actually supplied by us
* it's inside of our test functions, the sentence inside of the `"""`
	* underneath the declaration of the function
	* that is known as a Python docstring
		* they can serve as just a comment, probably describing what the function does
		* but if someone looking at the function, they actually can access that docstring
			* usually used for documentation
			* but they can be used in other places as well
				* in `unittest` case, it uses that docstring as a description of what the test is testing for
				* so we know what does it means something failed

so at the end of the day, what's wrong apparently is that, our `is_prime` function does checks but excluding the square root of `n`
* the fix would be
```python
import math

def is_prime(n):
	if n < 2:
		return False
	for i in range(2, int(math.sqrt(n) + 1)):
		if n % i == 0:
			return False
	return True
```

then we can run the test again
```bash
python3 test.py
......
--------------------
Ran 6 tests in 0.000s

OK
```

> [!warning] Test needs to be comprehensive 全面
> Automated tests only work, if our tests have good coverage of all the things that we would want the function to do. If the tests aren't comprehensive, unexpected behavior can pass the tests, and then break things.

___

# Django Testing
eg. let's apply some testing techniques in our airline application

`models.py`
```python
class Flight(models.Model):
	origin = models.ForeignKey(Airport, on_delete=models.CASCADE, related_name="departure")
	destination = models.ForeignKey(Airport, on_delete=models.CASCADE, related_name="arrival")
	duration = models.IntegerField()
	
	def __str__(self):
		return f"{self.id} : {self.origin} to {self.destination}"
	
	def is_valied_flight(self):
		return self.origin != self.destination or self.duration >= 0
```

`is_valied_flight(self)`
* we check to make sure the flight is an valid flight
* but testing only on `Flight` isn't enough, because other models have access to flights as well
	* there are some relationships going on around `Flight`, and we want to test them as well

remember when ever we create an application in Django, we get this `tests.py`
* it is used for writing these sorts of tests
```python
class FlightTestCase(TestCase):
	
	def setup(self):
		
		# Create airports
		a1 = Airport.objects.create(code="AAA", city="City A")
		a2 = Airport.objects.create(code="BBB", city="City B")
		
		# Create flights
		Flight.objects.create(origin=a1, destination=a2, duration=100)
		Flight.objects.create(origin=a1, destination=a1, duration=200)
		Flight.objects.create(origin=a1, destination=a2, duration=-100)
	
	# if this test works, we can be confident about <Airport>.departures.count()
	def test_departures_count(self):
		a = Airport.objects.get(code="AAA")
		self.assertEqual(a.departures.count(), 3)
	
	# similarly, this is for <Airport>.arrivals.count()
	def test_arrivals_count(self):
		a = Airport.objects.get(code="AAA")
		self.assertEqual(a.arrivals.count(), 1)
	
	# so the last 2 tests, test for relationships
	# this one tests the is_valid_flight function
	def test_valid_flight(self):
		a1 = Airport.objects.get(code="AAA")
		a2 = Airport.objects.get(code="BBB")
		f = Flight.objects.get(origin=a1, destination=a2, duration=100)
		self.assertTrue(f.is_valid_flight())
	
	# still testing the is_valid_flight function,
	# but test if it can detect invalid destination
	def test_invalid_flight_destination(self):
		a1 = Airport.objects.get(code="AAA")
		f = Flight.objects.get(origin=a1, destination=a2)
		self.assertFalse(f.is_valid_flight())
	
	# same, but test for detecting invalid duration
	def test_invalid_flight_duration(self):
		a1 = Airport.objects.get(code="AAA")
		a2 = Airport.objects.get(code="BBB")
		f = Flight.objects.get(origin=a1, destination=a2, duration=-100)
		self.assertFalse(f.is_valid_flight())
	
	...
```

`class FlightTestCase(TestCase)`
* the idea here is very similar to the `unittest`
* we define all of the tests that we would like to run, on our `flights` application

first thing to do is we might need some initial set-up
* so Django has some data that it can actually work with and test with
* that is what the `setup` function is doing
	* which is a special function, that Django knows it should run this function first before doing any test

when we run these tests, Django will create an entirely separate database for us
* just for testing purposes
* so we have one database, which is the real one, contains real information about real flights, on our web server
* but we also have another database, containing some dummy flights and dummy airports
	* just for testing uses
* then when we make sure that things are working, we can go ahead and deploy our web application, and let actual users begin to use whatever new features we've added to the web

then finally, how to run the tests?
```bash
python3 manage.py test
```

and what's the result?
* we get two fails
	* `FAIL: test_invalid_flight_destination`
	* `FAIL: test_invalid_flight_duration`

then we know the function `is_valid_flight` is some how wrong
* hopefully some time soon we can figure out why
* we used `or` instead of `and`
```python
def is_valid_flight(self):
	return self.origin != self.destination and self.duration > 0
```
* now we run the test again, and it outputs all OK

notice when we run the test, it said
`Creating test database for alia 'default'...`
* meaning literally, it just created a test database for doing all this testing work

and then at the end of the testing, it said
`Destroying test database for alias 'default'...`
* so there was the test database
	* it would do adding data, removing data, in there
* but at the end of the day, everything is cleaned up
* and it's not going to touch any of the actual data
___

## Simulate Requests

but testing Django models doesn't seem quite enough
especially since we're talking about web application
* because we would like to check if the web pages work
	* in particular do they work the way we expected them to

for that, Django can simulate making requests and getting responses

`test.py`
```python
class FlightTestCase(TestCase):
	...
	
	# Test the default flight page, in other words, the index page
	def test_index(self):
		c = Client()
		response = c.get("/flights/")
		self.assertEqual(response.status_code, 200)
		self.assertEqual(response.context["flights"].count(), 3)
	
	# Test getting a flight
	def test_valid_flight_page(self):
		a1 = Airport.objects.get(code="AAA")
		# the flight is invalid because its origin and destination are the same,
		# but it has an id any way, so it doesn't matter in this test
		f = Flight.objects.get(origin=a1, destination=a1)
		
		c = Client()
		response = c.get(f"/flights/{f.id}")
		self.assertEqual(response.status_code, 200)
	
	# Test getting a flight that doesn't exist
	def test_invalid_flight_page(self):
		# get the largest flight id
		max_id = Flight.objects.all().aggregate(Max("id"))["id__max"]
		
		c = Client()
		response = c.get(f"/flights/{max_id + 1}")
		self.assertEqual(response.status_code, 404)
	
	# Test getting passengers on a flight
	def test_flight_page_passengers(self):
		f = Flight.objects.get(pk=1)
		p = Passenger.objects.create(first="Alice", last="Adams")
		f.passengers.add(p)
		
		c = Client()
		response = c.get(f"/flights/{f.id}")
		self.assertEqual(response.status_code, 200)
		self.assertEqual(response.context["passengers"].count(), 1)
	
	# the last test is test getting non-passengers on a flight
	...
```

note that we can actually have multiple assert statements in one test
* in the case of our index page, for that page to work, it means the response status code should be `200` and it should list out all of the flights on the page
	* we created 3 dummy flights inside of this test database, in the `setup` function, so the context should have 3 flights

if you don't know what `response.context` is, recall when rendering a template, we can pass *context*, which is a Python dictionary, into the template and render things accordingly
* Django's testing framework gives us access to that context, so we can test it and make sure it contains the correct stuffs

also note that inside the test we can manipulate the database as well
* in our example it happens when we added some sample passengers
___

# Browser Testing

now we move our attention from the server to the front end
* we start from making a new page

`counter.html`
* like the `counter` app as before
	* we can increase the counter by click a button
* but this time we can both increase and decrease the counter
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Counter</title>
		<script>
			document.addEventListener('DOMContentLoaded', () => {
				let counter = 0;
				
				document.querySelector('#increase').onclick = () => {
					counter++;
					document.querySelector('h1').innerHTML = counter;
				};
				
				document.querySelector('#decrease').onclick = () => {
					counter--;
					document.querySelector('h1').innerHTML = counter;
				};
			});
		</script>
	</head>
	<body>
		<h1>0</h1>
		<button id="increase">+</button>
		<button id="decrease">-</button>
	</body>
</html>
```
___

## Selenium

we actually need to introduce a framework for this
* one of the popular is Selenium
* long story short, it can simulate a web browser
	* we can define a test file, that using `unittest` or some similar library
		* in this example we will call it `tests.py`
	* then it can simulate the user interactions
		* using something that web call a `webdriver`
* so we can use code to control, what it is that the browser is doing, and how it is that the user is interacting

to run the test, again we will use the Python interpreter
```python
from tests import *
```
* because we're using the `webdriver`, it will open up our web browser, and control it
	* we can see that because our web browser will tell us something like, "Chrome is being controlled by automated test software."
* then, we need to tell the browser to open up our web page
	* we can do that by getting our page's URI, *Uniform Resource Identifier*
	* for that, we have a function called `file_uri`
```python
uri = file_uri("counter.html")  # get our page's URI
driver.get(uri)  # tell the webdriver to open up our page
```
* then we can see our browser does indeed open up the page
	* notice that we're effectively controlling our web browser window using the Python program
* but let's pretend we didn't see the page opened up, and we want to see if our page's content are there, if they are loaded, via code
```python
driver.title
# output: 'Counter'
driver.page_source
# output: <html lang="en"><head><title>Counter</title></head><script>.....</html>
```
* next, we want to simulate user interactions
	* eg. like clicking on the + button
```python
driver.find_element_by_id('increase')
# output: <selenium.webdriver.remote.webelement.WebElement (session="...", element="...")>
increase = driver.find_element_by_id('increase')  # store the element found
# output:
increase.click()
# output:
```
* note that this is one of the reason why it's helpful to give elements an id
	* in case we ever need to be able to find that element
	* in this case we want to find that element because we want to test it
* also note that, `increase.click()` output nothing, but it does click the button
	* and we can see that our page responsed accordingly
* now we can push that a little bit further
```python
for i in range(25):
	increase.click()
```
* then we can see it happening and very quickly, the button is clicked indeed 25 times

likewise, testing the decrease button is the same thing
```python
decrease = driver.find_element_by_id("decrease")
decrease.click()
for i in range(24):
	decrease.click()
```

now you can start to see the power of that
* because imagine what if we want to click the button 100 times?
* no human could ever have clicked the button over and over again faster than the computer
* so it is not only can be automated, but also much faster than any human
___

## Automated Tests
it happens in the `tests.py`

```python
... # import the modules

def file_uri(filename):
	return pathlib.Path(os.path.abspath(filename)).as_uri()

driver = webdriver.Chrome()

class WebpageTests(unittest.TestCase):
	
	# test if the page's title is Counter
	def test_title(self):
		driver.get(file_uri("counter.html"))
		self.assertEqual(driver.title, "Counter")
	
	# test clicking the increase button
	def test_increase(self):
		driver.get(file_uri("counter.html"))
		increase = driver.find_element_by_id("increase")
		increase.click()
		self.assertEqual(driver.find_element_by_tag_name("h1").text, "1")
	
	# test clicking the decrease button
	def test_decrease(self):
		driver.get(file_uri("counter.html"))
		decrease = driver.find_element_by_id("decrease")
		decrease.click()
		self.assertEqual(driver.find_element_by_tag_name("h1").text, "-1")
	
	# test click the increase button 3 times
	def test_multiple_increase(self):
		driver.get(file_uri("counter.html"))
		increase = driver.find_element_by_id("increase")
		for i in range(3):
			increase.click()
		self.assertEqual(driver.find_element_by_tag_name("h1").text, "3")
	
	if __name__ == "__main__":
		unittest.main()
```

`file_uri`
* we need the URI to open up our web page

`webdriver.Chrome()`
* note that Chrome and ChromeDriver are different thing
	* we need to get that separately
* likewise, other web browsers make equivalent web drivers available as well
* here we only do tests on Google Chrome, but surely you may want to do tests on other web browsers as well

`WebpageTests(unittest.TestCase)`
* pretty much the same as the other test cases we have seen before
* it contains and defines all the tests

`driver.find_element_by_tag_name("h1").text`
* pretty much the same as `innerHTML`

and we will run these test cases by running
```bash
python3 tests.py
```
* then we can see very quickly all of those tests happening
	* probably faster than we human can comprehend
* and we can see our terminal outputs
```bash
....
--------------
Ran 4 tests in 0.721s

OK
```

and now let's intentionally make a bug in the decrease button, just to see what that output would look like
```bash
F...
==================
FAIL: test_decrease (__main__.WebpageTest)
------------------
Traceback (most recent call last):
	File "tests.py", line 31, in test_decrease
		self.assertEqual(driver.find_element_by_tag_name("h1").text, "-1")
AssertionError: '1' != '-1'
- 1
+ -1
? +



-------------------
Ran 4 tests in 0.831s

FAILED (failures=1)
```
___

# `unittest` Methods
these are some different `unittest` assertions

`assertEqual`
* assert two things are equal to each other

`assertNotEqual`
* assert two things are not equal

`assertTrue`
`assertFalse`
* assert something as true or false

`assertIn`
`asertNotIn`
* assert that some element is in or not in some list

....
___
