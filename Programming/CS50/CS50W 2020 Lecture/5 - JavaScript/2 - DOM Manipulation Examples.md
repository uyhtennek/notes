# Change CSS

`colors.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Colors</title>
		<script>
			document.addEventListener('DOMContentLoaded', function() {
				// Change font color to red
				document.querySelector('#red').onclick = function() {
					document.querySelector('#hello').style.color = 'red';
				};
				
				// Change font color to blue
				document.querySelector('#red').onclick = function() {
					document.querySelector('#hello').style.color = 'blue';
				};
				
				// Change font color to green
				document.querySelector('#red').onclick = function() {
					document.querySelector('#hello').style.color = 'green';
				};
			});
		</script>
	</head>
	<body>
		<h1 id="hello">Hello!</h1>
		<button id="red">Red</button>
		<button id="blue">Blue</button>
		<button id="green">Green</button>
	</body>
</html>
```

`document.querySelector('#hello').style`
* inside the `.style` property, we can modify any of the CSS style properties
	* eg. `color`
___

# Data attribute

you can see there are some copy and paste happening, so there is bad design
* it seems like the three buttons should use the same function, instead of separate functions
	* because they are so similar
* but if we do that, how do a button knows which color to change to?

for that, we can add a special attribute to the buttons, which is the *data attribute*

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Colors</title>
		<script>
			document.addEventListener('DOMContentLoaded', function() {
				document.querySelectorAll('button').forEach(function(button) {
					button.onclick = function() {
						document.querySelector('#hello').style.color = button.dataset.color;
					}
				});
			});
		</script>
	</head>
	<body>
		<h1 id="hello">Hello!</h1>
		<button data-color="red">Red</button>
		<button data-color="blue">Blue</button>
		<button data-color="green">Green</button>
	</body>
</html>
```

`<button data-color="red">`
* a data attribute always start with `data-`, then followed by any name that we want
* and it can store some information about te HTML element
	* in our case, it's what color to change the text to, when the button is clicked on
* and then if we access to this element, we have access to this `data-color` property as well

`document.querySelectorAll('button').forEach()`
* it's much like `document.querySelector`, except it selects all the elements that match the query
* it returns an array of matching elements

* we can try comparing them in Developer Tools > Console tab
	* `document.querSelector('button')` gives us the first `<button>` element
	* `document.querySelectorAll('button')` gives us a `NodeList`
		* it's something kinda like an array or a list in Python
		* it has three buttons in it
* we can index into an array, using something like `names[0]`
* then `names.length` to get the length of the array

* `.forEach` is a function, which accepts as an argument, another function
	* the idea is, "I would like to run a function, for each of the element inside of an array."
	* the function to run takes one of the elements in the array or list, as input
		* which is the current iterating one in the for each loop

`button.dataset.color`
* `dataset` is a special property of an HTML element
* `dataset.color` gives us the `data-color` attribute in the HTML element

> [!tip] Console
> When ever in doubt, we can always find help from the powerful Console, because we can write and run code in it. We can:
> * modify variables
> 	* create variables
> 	* update existing variables
> * run functions
> * run `document.querySelector` to figure out what elements are going to come back
> * ....

___

## Arrow notation
because `function()` is too much to write

```html
<!DOCTYPE html>
<script>
	document.addEventListener('DOMContentLoaded', () => {
		document.querySelectorAll('button').forEach(button => {
			button.onclick = () => {
				document.querySelector('#hello').style.color = button.dataset.color;
			}
		});
	});
</script>
```
___

# More Event Handlers


## `onchange`
eg. we want a drop down menu instead of three buttons

generally it means, when something changes
* when applied to a select drop-down, it means when the user chooses something different

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Colors</title>
		<script>
			document.addEventListener('DOMContentLoaded', function() {
				document.querySelector('select').onchange = () => {
					document.querySelector('#hello').style.color = this.value;
				};
			});
		</script>
	</head>
	<body>
		<h1 id="hello">Hello!</h1>
		<select>
			<option value="black">Black</option>
			<option value="red">Red</option>
			<option value="blue">Blue</option>
			<option value="green">Green</option>
		</select>
	</body>
</html>
```

`this.value`
* in JavaScript, we have access to a special value, `this`, and it has special meaning in JavaScript
* and its meaning varies, based on context
* in the context of an event handler, so when it is inside of a function that gets fired by an event, `this` refers to the thing that received the event
	* which is the `<select>` element in this case


## other events

`onclick`
* when something is clicked

`onmouseover`
* when the mouse is over something, or when it is hovering over some elements

`onkeydown`
* when we press down a key on the keyboard

`onkeyup`
* when we lift our finger off the key, so the key get up

`onload`

`onblur`

....
___

# eg. To-Do List
but this time we do it using exclusively JavaScript

`tasks.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Tasks</title>
		<script>
			document.addEventListener('DOMContentLoaded', function() {
				// we want the submit button is disabled when the input field is empty
				// so the user can't submit an empty task, so it's more user friendly
				
				// by default, disable the submit button, because it's empty
				document.querySelector('#submit').disabled = true;
				
				document.querySelector('#task').onkeyup = () => {
					// when the user types something, enable the submit button
					if (document.querySelector('#task').value.length > 0) {
						document.querySelector('#submit').disabled = false;
					} else {
						document.querySelector('#submit').disabled = true;
					}
				}
				
				document.querySelector('form').onsubmit = () => {
					const task = document.querySelector('#task').value;
					console.log(task);  // not necessary, for example only
					
					const li = document.createElement('li');
					li.innerHTML = task;
					
					document.querySelector('#tasks').append(li);
					
					// clear the input field so it's more user friendly
					document.querySelector('#tasks').value = '';
					// disable the submit button again
					document.querySelector('#submit').disabled = true;
					
					// Stop form from submitting
					return false;
				}
			});
		</script>
	</head>
	<body>
		<h1>Tasks</h1>
		<ul id="tasks">
		</ul>
		<form>
			<input id="task" placeholder="New Task" type="text">
			<input id="submit" type="submit">
		</form>
	</body>
</html>
```

`console.log(task)`
* it logs something on the Console tab
* equivalent to `print()` in Python

`return false`
* it prevents the form from submitting
* so it doesn't make a request to our server
___

# Intervals

thus far, all the events introduced involve user interactions
* except `'DOMContentLoaded'`
* but we can have functions that run on their own

one way is that we can set an event that get triggered every some number of milliseconds

`count.js`
```js
let counter = 0;

function count() {
	counter++;
	document.querySelector('h1').innerHTML = counter;
}

document.addEventListener('DOMContentLoaded', function() {
	document.querySelector('button').addEventListener('click', count);
	
	setInterval(count, 1000);
});
```

`setInterval(count, 1000)`
* this is a function built into JavaScript
* in this case, it said, "every `1000` milliseconds, run the `count` function"
	* 1,000 milliseconds = 1 second
___
