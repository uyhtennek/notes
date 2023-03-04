# Client-Side Programming

Thus far, Python and Django are things that happen on the server
JavaSciprt, on the other hand, allows us to do computation on the client

why we would ever want that?
* some code we don't need it to reach out to our server
	* then we can just run the code on the client
	* then it's potentially faster
* we can begin to make our web pages a whole lot more interactive
	* we can directly manipulate the [[1 - HTML#DOM|DOM]] via JavaScript
___

# `<script>`
this tag tells the browser to interpret anything inside of it, as JavaScript code

```html
<script>
	alert('Hello, world!');
</script>
```

`hello.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Hello</title>
		<script>
			// string in JavaScript can use either "" or ''
			alert('Hello, world!');
		</script>
	</head>
	<body>
		<h1>Hello!</h1>
	</body>
</html>
```
___

# Events

one big area where JavaScript can be quite powerful, is that
JavaScript is an *event-driven programming*
* we would think of things that happen on the web, as *events*
* eg. an event can be a user clicks on a button, a user selects something from a drop-down list, or a user scrolls through a list, or submits a form
	* basically anything the user does

What we can do with JavaScript, is that
we can add *event listeners* or *event handlers*
* they get triggered when a particular event happen, and then run a particular block of code

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Hello</title>
		<script>
			
			function hello() {
				alert('Hello, world!');
			}
			
		</script>
	</head>
	<body>
		<h1>Hello!</h1>
		<button onclick="hello()">Click Here!</button>
	</body>
</html>
```

`<button onclick="hello()">`
* the `onclick` attribute defines what should happen when the button is clicked
___

# Store Variables

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Count</title>
		<script>
			
			let counter = 0;
			
			function count() {
				counter++;
				alert(counter);
			}
			
		</script>
	</head>
	<body>
		<h1>Hello!</h1>
		<button onclick="count()">Count</button>
	</body>
</html>
```
___

# Selector

these along surely aren't powerful
* especially if our only way to do interactions is via that alert thing, it would be annoying

things can more interesting when we can update the content of the web page
* since JavaScript can update the DOM

eg. on clicking a button, we want to change the `<h1>` heading of `Hello!`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Hello</title>
		<script>
			
			function hello() {
				document.querySelector('h1').innerHTML = 'Goodbye!'
			}
			
		</script>
	</head>
	<body>
		<h1>Hello!</h1>
		<button onclick="hello()">Click Here!</button>
	</body>
</html>
```

`document.querySelector('h1')`
* it looks through an HTML page, and extract an element from it, so we can then manipulate that HTML element, using JavaScript
* `querySelector` is only going to return one element
	* if there are multiple, it returns the first one it finds
___

## Conditions

we can use conditions in JavaScript
```html
<script>
	function hello() {
		if (document.querySelector('h1').innerHTML === 'Hello!') {
			document.querySelector('h1').innerHTML = 'Goodbye!';
		} else {
			document.querySelector('h1').innerHTML = 'Hello!';
		}
	}
</script>
```

`===`
* it's JavaScript's way to check for *strict equality*
* checking if two values are equal, and also their types are the same thing
* `==` is a weaker check for equality
	* it just checks the values, but not the types
* usually, if you can, you would want to use `===`
___

## Store Elements in Variables

as you can imagine, it's not a very efficient code
* each time we run `document.querySelector`, it looks through the page to find an element
* we used it three times, but just to find the same element

```html
<script>
	function hello() {
		const heading = document.querySelector('h1');
		if (heading.innerHTML === 'Hello!') {
			heading.innerHTML = 'Goodbye!';
		} else {
			heading.innerHTML = 'Hello!';
		}
	}
</script>
```

`const`
* it enforces the variable is never going to change
	* it is good design because it makes it clear that the variable is not supposed to be changed
	* it will give us an error if we do try to change it
* so if we're not going to change its value inside of the scope of the function, make it `const`
* if we do want to change it, we can use `let`
___

## eg. improve `Counter.html`

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Count</title>
		<script>
			
			let counter = 0;
			
			function count() {
				counter++;
				document.querySelector('h1').innerHTML = counter;
				
				if (counter % 10 === 0) {
					alert(`Count is now ${counter}`);
				}
			}
			
		</script>
	</head>
	<body>
		<h1>0</h1>
		<button onclick="count()">Count</button>
	</body>
</html>
```

\``Count is now ${counter}`\``
* this is what JavaScript calls a *template literal*
* it's like formatted string in Python
	* so it is the equivalent of `f"Counter is now {counter}."`, if it was Python
___

# Adding Event Listener
in JavaScript

although we can add the `onclick` attribute in the HTML element directly to add an event listener, sometimes we would like to separate all of the JavaScript code from all of the HTML
* eg. if we want to change the function name, we need to change it in both JavaScript and HTML
* actually there are other paradigms that encourage making use of HTML attributes
	* for now, we want to separate them, for better design

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Count</title>
		<script>
			
			let counter = 0;
			
			function count() {
				counter++;
				document.querySelector('h1').innerHTML = counter;
				
				if (counter % 10 === 0) {
					alert(`Count is now ${counter}`);
				}
			}
			
			document.querySelector('button').onclick = count;
			
		</script>
	</head>
	<body>
		<h1>0</h1>
		<button>Count</button>
	</body>
</html>
```

note that it's `count` but not `count()`
* because `count()` would be to run the function
* then the value gets returned to `onclick` would be the return value `count()`, instead of the function `count`
* so notice that, functions in JavaScript are much like variables, which have a name and a value
	* it's a paradigm we generally call as *functional programming*

however, if we open up the page now, it would give us an error
* we can open up the Developer Tool and then look into the Console tab, where all the messages from our browser rest in
* it says something like "can't modify the `onclick` property of `null`."
* extra. we can write and execute JavaScript code here as well

It turns out, `document.querySelector` would return `null` if it is unable to find something
* why is that? because browser reads an HTML file, top to bottom, line by line
* when it reads `document.querySelector('button')`, and execute that, the `<button>` element doesn't exist yet, since it is further down in the page
	* it is not loaded in the DOM yet, so we won't find it

so solution one is obvious, we can just take the `<script>` and place it at the end of the page
but there is another solution, perhaps a more common solution
* which is adding yet another event listener

```html
<script>
	
	let counter = 0;
	
	function count() {
		counter++;
		document.querySelector('h1').innerHTML = counter;
		
		if (counter % 10 === 0) {
			alert(`Count is now ${counter}`);
		}
	}
	
	document.addEventListener('DOMContentLoaded', function() {
		document.querySelector('button').addEventListener('click', count);
	});
	
</script>
```

`document.addEventListener()`
* `document` is a variable built into JavaScript, and it gives access to us to the web page
	* it refers to the entire web document
	* so `document.querySelector` means finding something from the entire web document
* `addEventListener` is a function we can add to any HTML element
	* not just the `document`
	* generally it taks two arguments
		* the first one is the event we want to listen for
			* eg. `click`, when the user scroll through a page, etc.
			* `DOMContentLoaded` is an event that will get fired when the DOM is done loading
				* when all of the elements on the page, the DOM content, are done loading
		* the second argument is the function or the code to run when the event happens
			* and we can use anonymous function for that

`document.querySelector('button').onclick = count`
`document.querySelector('button').addEventListener('click', count);`
they both work, and have the same effect
* they don't work exactly the same though
___

# JavaScript file
much like the case for CSS, often times we prefer to separate JavaScript from the HTML file entirely

`counter.js`
```javascript
let counter = 0;

function count() {
	counter++;
	document.querySelector('h1').innerHTML = counter;
	
	if (counter % 10 === 0) {
		alert(`Count is now ${counter}`);
	}
}

document.addEventListener('DOMContentLoaded', function() {
	document.querySelector('button').addEventListener('click', count);
});
```

`counter.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Count</title>
		<script src="counter.js"></script>
	</head>
	<body>
		<h1>0</h1>
		<button>Count</button>
	</body>
</html>
```

then if we have a common JavaScript code that's in use by multiple different HTML pages, the HTML pages can all include the same JavaScript source
* so we avoided repeating ourselves
* eg. we often times will use some JavaScript libraries
	* like Bootstrap
___

# eg. Interactive Hello

`hello.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Hello</title>
		<script>
			document.addEventListener('DOMContentLoaded', function() {
				document.querySelector('form').onsubmit = function() {
					const name = document.querySelector('#name').value;
					alert(`Hello, ${name}!`);
				};
			});
		</script>
	</head>
	<body>
		<h1>Hello!</h1>
		<form>
			<input autofocus id="name" placeholder="Name" type="text">
			<input type="submit">
		</form>
	</body>
</html>
```

`document.querySelector('#name')`
* it turns out `querySelector` in JavaScript is much like [[2 - CSS#Selectors|selectors in CSS]]
	* except, it returns only one, or the first, element
* `document.querySelector('tag')`
* `document.querySelector('#id')`
* `document.querySelector('.class')`
___
