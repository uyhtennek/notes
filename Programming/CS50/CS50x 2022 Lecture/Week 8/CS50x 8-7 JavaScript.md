# JavaScript
* quite similar syntactically to C and Python
* indeed if we can do something in C or Python, we can probably do it in some form in JavaScript

The most fundamental difference is
* C and Python, we thus far wrote them on a server, in the terminal window environment
	* also when we run the code, it's running on the server
* JavaScript though, we still would write it in the cloud
	* but when we execute the code, it's running in our browser, not in the server
	* recall when we load a web page, it's essentially is a [[CS50x 8-6 CSS#Developement Tools|copy]]
	* Although, there is an environment called **Node.js** that do allows us to run JavaScript on the server. It's an alternative to Python or Ruby or Java or others.
* So this time we execute program on client side, instead of server side

btw, it has nothing to do with Java
* similar name, but no relation
___

# JavaScript Syntax

* declare a variable
```javascript
let counter = 0;
```
use `let` for any type of data
JavaScript would figure out what type it should use

* increment by 1
```js
counter = counter + 1;
counter += 1;
counter ++;
```

* conditional
```js
if (x < y)
{

}
else if (x > y)
{

}
else
{

}
```
pretty much like C

* loop forever
```js
while (true)
{

}
```
Booleans are lowercase again, like in C

* loop 3 times
```js
for (let i = 0; i < 3; i++)
{

}
```

That's pretty much it.
There are other features, but these are the main things.
___

# JavaScript and HTML

The power of running JavaScript on a user's browser,
is that we can change the HTML in memory.
* also it implies that CSS or style of a HTML, too, can be changed using JavaScript

Think of some interesting website or web pages.
They are typically very interactive and dynamic.
* eg. when we are checking Gmail, what happen if someone sent a email to us at the time
	* another row appears in the inbox
		* but we don't need to hit Ctrl + R to reload the page, to see more email
		* it automatically appears
	* the inbox is probably just a HTML `<table>`, maybe it's a bunch of `<div>`
* how is that possible the HTML get changing in real-time?

When we visit Gmail.com, we're downloading not just HTML and CSS, and our initial inbox
But also some JavaScript code
* That is designed to keep looking for new emails every second, every 10 seconds or something
	* keep looking at the Gmail server
* If there are something new, the code would add some elements
	* if there are a new email again, the code yet add another element
* The elements are added to the existing DOM, *document object model*
	* it's a fancy term for tree in memory that represents HTML
* So that the web page can continue to update in real time

eg. Google Map
* our browser actually doesn't download the entire world map, by default
* so sometimes when we drag and drag and drag the map, we can see the map loading
	* or more specifically it's loading the tiles
* meaning only the places, the street names, restaurants in our viewport, get downloaded
* behind the scene it's JavaScript again
___

# Implementation of JavaScript

now let's implement some JavaScript to our HTML
```html
<!DOCTYPE html>

<html lang="en">
	<head>
		<script>
			
		</script>
		<title>hello, title</title>
	</head>
	<body>
		hello, body
	</body>
</html>
```

we use `<script>` to encapsulate our JavaScript code
* the codes, if any, would be placed at the `<head>`
	* but in fact it can be placed in the `<body>`
* but typically we would place the JavaScript in a separate file instead
	* same reason as [[CS50x 8-6 CSS#Linking CSS|separating CSS file]]
	* using the line `<script scr="scripts.js"></script>`


1. Add a button

* Currently the HTML really does nothing except just showing `hello, body`
* We didn't prepare anything for the user to interact with
	* so let's place a text box for user to type his name and a button to trigger the code to run
	* just copy from [[CS50x 8-5 URL parameter#Implementing a search feature|search.html]]

```html
...
	<body>
		<form>
			<input autofocus id="name" placeholder="Name" type="text">
			<input type="submit">
		</form>
	</body>
...
```


2. `onsubmit`

* Let's introduce the messy way first

```html
...
	<head>
		<script>
		
			function greet()
			{
				alert('hello, there');
			}
		
		</script>
		<title>hello</title>
	</head>
	<body>
		<form onsubmit="greet(); return false;">
			<input autofocus id="name" placeholder="Name" type="text">
			<input type="submit">
		</form>
	</body>
...
```

`<form onsubmit="greet(); return false;">`
* on the form is submitted, run the code `"greet(); return false;"`
* `"greet()"` need to be defined in the `<script>`
* When the user clicks submit, normally the form get submmitted to the server, but this time we don't want to do that
	* we just want to submit the form to the browser, not the server
	* we just need it to print something on the screen
	* that's why we need the `return false;`
		* it tells the browser, even when the user submits the form, `return false`
		* so don't let them actually submit the form
	* but do call that function called `greet()` anyway

`<script>`
* inside the tag, we wrote some JavaScript code
* `function greet()` shows how we would define a function in JavaScript
	* we use the word `function` instead of `def` like in Python
	* also like in Python, we don't specify a return type
* `alert()` is a built-in function
	* which is not a good user interface
	* but we're taking baby steps here


3. Selector in JavaScript

Right now the code has nothing to do with the name that the user typed in the text box
So let's grab that text.
* notice the text box element is given an `id` already
* maybe we can grab the value of that text box using the `id` property
	* like how it's done [[CS50x 8-6 CSS#ID selector|in CSS]]

```html
...
	<head>
		<script>
		
			function greet()
			{
				let name = document.querySelector('#name').value;
				alert('hello, ' + name);
			}
		
		</script>
		<title>hello</title>
	</head>
...
```

`document.querySelector().value`
* `document.querySelector()` is the JavaScript version of [[CS50x 8-6 CSS#^3b761b|selectors]]
* to select nodes, or elements
	* using hashes `#` or dots `.` or other syntax
	* using the same syntax as CSS's
* `'#name'`
	* that gives us the actual node from the tree
	* one of the element from the DOM, *document object model*
* `.value`
	* get the value of the thing that we selected
	* similar to Python, where it too uses dots `.`, for going inside of [[CS50x 6-2 C and Python#^fe352a|objects]]
	* in here it's the same story

Long story short, `document` is a special global variable in JavaScript
* that lets us just do stuff with the document, the web page itself, the HTML
* one of its function is `querySelector()`
	* which returns to us whatever it is that we're selecting
* `.value` means go inside of that element and grab the inputted text
___

# Event Listeners

However, right now our code is in the `onsubmit` attribute,
which is not ideal because it can't really scale
* So let's remove that
* actually never use `onsubmit` ever again

```html
...
	<head>
		<title>hello</title>
	</head>
	<body>
		<form onsubmit="greet(); return false;">
			<input autofocus id="name" placeholder="Name" type="text">
			<input type="submit">
		</form>
		<script>
			
			function greet()
			{
				let name = document.querySelector('#name').value;
				alert('hello, ' + name);
			}
			
			document.querySelector('form').addEventListener('submit', greet);
			
		</script>
	</body>
...
```

`<script>`
* notice, [[#Implementation of JavaScript|before]] it's in the `<head>` tag, now we moved it to the `<body>` tag, under the `<form>` tag
	* so that logically `<script>` tag exists after `<form>` tag exists
	* like if `<form>` tag doesn't exist, where do we put the `<script>` tag right?
* however if it is preferred to place the `<script>` tag in the `<head>`, we can place a `defer` attribute to make the `<script>` load or execute after the `<body>` finished loading

`document.querySelector('form')`
* here we select directly the `'form'` element
	* it doesn't need a unique ID, we can just reference it by name, `'form'`
	* because in this case there is only one of them

`.addEventListener('submit', greet)`
* this is a function that listens for events
	* we pretty much use events any where
	* in many different languages
	* for any user interface, especially phones
		* on phones, we have touches, drags, long press, pinch, etc.
		* on Mac or PC, we have click, drag, key down, key up, etc.
	* recall even Scratch let us broadcast events
	* also in any field
		* web programming, game programming, etc.
	* any thing that allows a human to do something any time

> [!events in web]
> blur
> change
> click
> drag
> focus
> keyup
> load
> mousedown
> mouseover
> mouseup
> submit
> touchmove
> unload
> ...

* `'submit'` is the event that we want to listen for
* when the `'submit'` event happens, the `greet` function get called
	* notice we use `greet` instead of `greet()`
	* because `greet()` would call the function right a way
___

# Anonymous Functions

Notice we just defined a `greet` function but we only need it in one place
It feels like we can have a better design, if we just don't define this function
recall we did some [[CS50x 7-1 Playing with Data#Lambda function|similar thing]] in Python

In JavaScript, we have lambda functions, or anonymous functions, as well
* even better than Python, JavaScript allows lambda functions to have multiple lines of code.

```html
...
	<head>
		<title>hello</title>
	</head>
	<body>
		<form>
			<input autofocus id="name" placeholder="Name" type="text">
			<input type="submit">
		</form>
		<script>
			
			document.querySelector('form').addEventListener('submit', function() {
				let name = document.querySelector('#name').value;
				alert('hello, ' + name);
			});
			
		</script>
	</body>
...
```

However, if we want to block the form from being submitted, we need to do one thing
which is quite hard to discover, unless being told to or reading the documentation
* the function would take in a parameter `e`
* then we need to also add a line of code in the function
	* `e.preventDefault()`

```html
...
		<script>
			
			document.querySelector('form').addEventListener('submit', function(e) {
				let name = document.querySelector('#name').value;
				alert('hello, ' + name);
				e.preventDefault();
			});
			
		</script>
...
```

`e`
* a variable that represents the event
`e.preventDefault()`
* prevent whatever the default handling of that particular event is
___

# JavaScript Examples

1. 
```html
...
		<script>
			let body = document.querySelector('body');
			document.querySelector('#red').addEventListener('click', function() {
				body.style.backgroundColor = 'red';
			});
		</script>
...
```

it changes the `<body>` background color to `'red'`, when a button with property `id="red"` is clicked
Notice however in CSS the key for background color is `background-color`, but in JavaScript it's `backgroundColor`
* In CSS, properties that have two words are usually hyphenated, separated with a hyphen `-`
	* eg. `background-color`
* In JavaScript, `background-color` would be `background` subtracts `color`, which makes no sense
	* so the solution is captialize the second word
	* eg. `backgroundColor`, no minus sign / hyphen `-`

* Yeah by the time the people who developed CSS and the people who developed JavaScript, didn't talk to each other in advance, and some how there isn't a solution yet
	* now we just live with it....

2. 
back in the day, there used to be a `<blink>` tag
* it makes a text blinks...
it's a historical one because now it's gone, removed from HTML
but in the last '90s, early 2000s, the web is full of these ugly stuffs
something like a marquee ( 走馬燈 ), that would move text from right to left over the screen

although it's removed, we can create one on our own, how?
* write some code that waits every like 500 milliseconds to change the CSS of the page, or the text, to be visible, invisible, visible, invisible, visible.....
* JavaScript is built in with some clock functionality that we can use to do that, like making some sort of schedule

3. 
load some dictionary database or words database to memory
and create a auto-complete feature
* when it finds matching words, just update the DOM, the tree in the computer's memory
* to show more text, or less text

4. 
these days browers are built-in with some APIs
* API, *application programming interface*

One of them is asking for information about the user's device
* for instance its location
* getting its GPS coordinates
___

# Summary

HTML
* structure

CSS
* style

JavaScript
* logic
___
