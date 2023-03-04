# Variables

* similar to Python variables
* no type specifier
* declare [[C - 7 Variable Scope#Scope|local variable]] with the `var` keyword

In python, a local variable looks like
```python
x = 44
```

but in JavaScript, that would actually created a global variable
if we want a local variable we need the `var` keyword
```js
x = 44;      // global variable
var x = 44;  // local variable
```
___

# Conditionals

All C [[C - 4 conditional statements|conditionals]] are available
* `if`, `else if`, `else`
* `switch`
* `?:`
___

# Loops

All C [[C - 5 Loops|loops]] are available
* `while`
* `do-while`
* `for`

One more thing,
In JavaScript we can actually iterating through properties for an object
* iterating across all of the key-value pairs of an object
* similar in spirit of [[CS50x 6-1 Python Beginner Tour#loop 3 times|iterating through a list]] in Python

```python
for key in list:
	# use key in here as a stand-in for list[i]
```

In JavaScript, this would iterate across all keys in an object
```js
for (var key in object)
{
	// use object[key] in here
}
```

similarly, we also can do iterating across all the values of an object
```js
for (var val of object)
{
	// use val in here
}
```

eg.
```js
var wkArray = ['Monday', 'Tuesday', 'Wednesday', 'Thursday',
			   'Friday', 'Saturday', 'Sunday']

for (var day in wkArray)
{
	console.log(day);
}
for (var day of wkArray)
{
	console.log(day);
}

// result
0
1
2
3
4
5
6
Monday
Tuesday
Wednesday
Thursday
Friday
Saturday
Sunday
```

* `console.log()` is like the `printf()` in C
	* we can access it using developer tools in most modern browsers
* since it's an array, key is not really a thing, so it prints out the indices instead
___

# Functions

* funnctions are introduced with the `function` keyword
```js
function myFunction()
{
	// some code
}
```

* functions can be *annonymous*
	* meaning we don't have to give them a name
	* quite often to use actually
* if the functionality is never used a second time, just make it *annonymous*
	* so we won't clutter up the name space
	* also it's kinda convenient to do so if we ever would just use the function one time only

```js
var nums = [1, 2, 3, 4, 5];

// this will double all numbers in `nums`
// in other word it would run this annonymous function once for each numbers in `nums`
nums = nums.map(function(num) {
	return num * 2;
});
```
___

# Arrays
actually array is not a thing in JavaScript

initialize an int array
```js
var nums = [1, 2, 3, 4, 5];
```

* we can also have mixed type array
```js
var mixed = [1, true, 3.333, 'five'];
```

However, actually array is not a thing in JavaScript
It's a special case of an object
* In fact, everything in JavaScript is a special case of an object
	* even as simple as `var x = 44;`, `x` is actually an object that happens to have just one property
* therefore there are some methods that we can apply to the "array" object
	* some major ones are `array.size()`, `array.pop()`, `array.push(x)`, `array.shift()`
	* they're built-in because those functionality is so baked into what it means to be an array
		* so we can just call them instead of writing something with the same functionality

`map()` is another very commonly used function of the "array" object
which allow us to apply a function to every single element of an array
* so this is a great place to use an *annonymous function* sometimes
___

# Objects

> [!quote]
> JavaScript is a little bit tricky as the very first language to learn,
> because it sets up a bucnh of rules for itself, and then breaks them.
> 
> It's a flexible language, but perhaps too flexible as a first language.

It can behave as an [[CS50x 6-2 C and Python#^fe352a|object-oriented programming language]]
* an object is similar in spirit to a [[C - 14 Structures|structure]] from C
	* in C, structure can have a number of difference fields or members
	* in the context of object-oriented object we would call them *properties*
	* properties can never stand on their own
* besides properties, an object also can have *method*
	* sort of like if in C a structure had a function
		* in C we can do `function(object);`, but not `object.function();`
		* `object.function();` is object-oriented styling
	* like properties, methods also can't stand on their own
	* they means nothing outside of the object

Also objects are kinda like [[Python Syntax#Dictionaries|dictionary]], which we may recall in Python
```js
var herbie = {year: 1963, mode: 'Beetle'};
```
___

# Concatenate variables

use plus `+` to concatenate strings
```js
console.log(wkArray[day] + ' is day number ' + (day + 1) + ' of the week!');

// result
Monday is day number 01 of the week!
```

however, the result would be unexpected
since by `day + 1` we mean add, but JavaScript performed string concatenation
* although `day` is already an integer

we need to use `parseInt(day) + 1` instead
* although JavaScript is flexible enough for us to not declare data type
* it sometimes guess wrong in what we intended to do
* so it also gives us ways to work around the wrong guess
___

# Events

An event, in the context of HTML and JavaScript, is a response to user interaction on a web page
* the user is doing something on a web page
* eg. a button is clicked, a page has finished loading as maybe the user hits Refresh for example, the mouse hovers over a portion of the page, the user typed in an input field, etc.

JavaScript has support for *event handlers*
* which is some JavaScript code that will execute once that event has taken place
* so it is very important to make a page interactive
* eg. once user typed the first letter in an input field, then pop up some label to tell the user to input his password

Many elements in HTML has support for events as an attribute
```html
...
	<body>
		<button onclick="">Button 1</button>
		<button onclick="">Button 2</button>
	</body>
...
```
* `onclick` makes some JavaScript code to happen when that button is clicked

We can write a generic event handler in JavaScript
* a function which takes an *event object* as a parameter
* which tells us which button was clicked

```js
function alertName(event)
{
	var trigger = event.srcElement;
	alert('You clicked on ' + trigger.innerHTML);
}
```

```html
...
	<body>
		<button onclick="alertName(event)">Button 1</button>
		<button onclick="alertName(event)">Button 2</button>
	</body>
...
```

1. when either Button 1 or Button 2 is clicked
2. the function `alertName(event)` get called
	* with the `event` passed in
	* which is automatically generated by the page
	* it basically contains all of the information about what just happened
3. In the function `alertName(event)`, there is a local variable `trigger`, storing `event.srcElement`
	* `srcElement` shorts for *source element*
	* `event.srcElement` is telling that what is the element that triggered this event
4. `alert()` to show a message that saying which button got clicked
	* `innerHTML` is what is between the tags
	* in this case what is between the `<button>` tags are `Button 1` or `Button 2`
___

# 4 ways to change color

```html
...
	<body>
		
		<div id="colorDiv">Change my color!</div>
		
		<!-- First way: simple function call -->
		<button onclick="turnGreen();">Green</button>
		
		<!-- Second way: function call with parameter -->
		<button onclick="changeColor('green');">Green</button>
		
		<!-- Third way: event handler -->
		<button onclick="changeColorEvent(event);">Green</button>
		
		<!-- Forth way: jQuery event handler -->
		<button class='jQButton'>Green</button>
		
	</body>
...
```

```js
// First way
function turnGreen()
{
	document.getElementById('colorDiv').style.backgroundColor = 'green';
}

// Second way
function changeColor(color)
{
	document.getElementById('colorDiv').style.backgroundColor = color;
	// or
	document.querySelector('#colorDiv').style.backgroundColor = color;
	// alternatively if you prefer jQuery
	$('#colorDiv').css('background-color', color);
}

// Third way
function changeColorEvent(event)
{
	var triggerObject = event.srcElement;
	document.getElementById('colorDiv').style.backgroundColor
	= triggerObject.innerHTML.toLowerCase();
}

// Forth way
$(document).ready(function() {
	$('.jQButton').click(function() {
		$('#colorDiv').css('background-color', this.innerHTML.toLowerCase());
	});
});
```
___
