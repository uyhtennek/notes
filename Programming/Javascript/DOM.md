# Introduction
**DOM**: *Document Object Model*

Recall JavaScript is a object-oriented programming language
where [[JavaScript Fundamentals#Objects|objects]] contain properties and methods
* and we can even put objects inside objects
* therefore with this nesting structure we can even create a [[CS50x 5-3 Tree|tree]] structure inside of a single object

The *document object* is a special object in JavaScript
that organizes the entire contents of a web page into it
* the whole HTML structure of a page is stored in it in some way
* therefore we can manipulate the page's elements using this object
	* we can edit the content of a web page in JavaScript
	* instead of going back and editing the HTML or the Python that underlies it

* We can drill down to some specific pieces or elements of our website
	* surely we need to know some related properties and methods for doing so
* We can reset those properties, or calling some certain methods, to change the contents of our web pages
	* without us needing to refresh the page
___

# Preview DOM in browser

Open the Browser's Developer Tools and get to the Console tab
* we can print message on it
* we can enter command to it to make the web do something

`console.dir(document)`
* `console.dir()` will organize the content of the page in a directory structure
	* let us preview the document object of the page
* we need to put as an argument what we want to see
	* in this case `document` basically means everything
	* so `document` is in fact the document object of the page

as we enter this command, the console will print a `#document` thing
as we expand it, there are a lot of stuffs there
but what we are interested here is the `children` property
* inside of it there is one child called `html`
* now if we pop that open, `html` also has `children`
	* typically they're `head` and `body`
___

# DOM properties
There are many properties inside of a doucment object, here are some major ones

Firstly here is our example HTML
```html
...
	<body>
		<p>Hello, world</p>
	</body>
...
```

`innerHTML`
* it holds what is between the HTML tags
* the `innerHTML` of the `<p>` element here would be `Hello, world`
* the `innerHTML` of the `<body>` element would be `<p>Hello, world</p>`

`nodeName`
* the name of an HTML element or element's attribute
* it would be `p` or `body` in our example

`id`
* the `id` attribute of an HTML element

`parentNode`
* a reference to the node one level up in the DOM
* in `<p>` case it would be the `<body>` node

`childNodes`
* an array of references to the nodes one level down in the DOM
* `<body>` node's `childNodes` would be an array with only the `<p>` node in it
* and if it was the `<p>` node the array would be empty

`attributes`
* an array of attributes of an HTML element

`style`
* an object encapsulating the CSS/HTML styling of an element
___

# DOM methods

`getElementById(id)`
* gets the element with a given ID
* only can find elements that are below, in the tree structure, of this node
* only return one element, the first one
* be careful that it's a capitalized `I` and a lower-case `d`, `Id`, not `ID`

`getElementsByTagName(tag)`
* gets all elements with the given tag
* also can only find elements that are below this node

`appendChild(node)`
* add the `node` below where the current node is
* allows us to add elements on the fly

`removeChild(node)`
* just the opposite of `appendChild(node)`
* remove the specified child node

One last thing is that, if we want to find something from the entire page
just start from the `document` node
___

# jQuery
[jQuery API Documentation](https://api.jquery.com/)

It's quite common to use a JavaScript library to do the DOM manipulation
* because the default JavaScript for doing so can get quite lengthy

jQuery is quite popular, probably the most popular
* open-source library
* released in 2006
* designed to simplify *client-side scripting*
	* such as DOM manipulations and AJAX queries

for example here is a pure JavaScript code to change a background color of an element
```js
document.getElementById('colorDiv').style.backgroundColor = 'green';
```

In jQuery, it's shorter, but it looks more weird
```js
$('#colorDiv').css('background-color', 'green');
```

`$`
* a shorthand way of saying jQuery
`$('#colorDiv')`
* look for the element with an ID `colorDiv`

`$('colorDiv').css()`
* then set that node's CSS or styling
___

# jQuery event handler

jQuery also offers us another way to do event handler
but it requires giving the elements an id or a class

```js
$(document).read(function() {
	$('.jQButton').click(function() {
		$('#colorDiv').css('background-color', this.innerHTML.toLowerCase());
	});
});
```

Here is what is happening
1. when the `document`, the page, is fully loaded, apply the following function inside of `read()`
2. jQuery finds all the elements with the class `jQButton` and make them run the following function inside of `click()`, when they are clicked
3. find the element with the id `colorDiv`, and change its css attribute `background-color`
	* to the color of the text between of this element tags
	* since CSS is case-sensitive so we need to change the text to all lower case

The equivalent for `$(document).read(function() {})` in pure JavaScript is `document.addEventListener('DOMContentLoaded', function() {});`
```js
document.addEventListener('DOMContentLoaded', function() {
	document.querySelectorAll('.jQButton').addEventListener('click', function() {
		document.querySelector('#colorDiv').style.backgroundColor = 
			this.innerHTML.toLowerCase();
	});
});
```
___
