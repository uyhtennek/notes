# CSS
*Cascading Style Sheets*

* HTML is used to organize the content of a web
* CSS is how the content actually looks

CSS is not a programming language
* just like HTML
* doesn't have variables, doesn't have logic, etc.
* It's a styling lanugage

Its syntax describes how the elements of our page should be modified
* by setting the elements' attributes

CSS is pretty simple in fact, that we can learn what the code does just by guessing
```css
body
{
	background-color: blue;
}
```

this example here, just set the `background-color`, of the `body` element in our page, to `blue`
* CSS is case-sensitive, so putting `Blue` instead of `blue` here is invalid
___

# Style sheet

a selector takes a style sheet, so a CSS file actually would contain multiple style sheets
a style sheet contains some major components

1. *selector*
	* that's the `body` in the last example
	* after that we put curly braces `{}` to begin defining the style sheet for that selector

2. style sheet
	* it means the curly braces `{}` and the key-value pairs inside
		* *declaration* is how we would call these key-value pairs
	* `background-color` is the key, `blue` is the value, in the last example
	* then each declaration ends with a semi-colon `;`
___

# Selector
There are different types of selecotrs

**tag selector**
* apply style to all elements with a given HTML tag
```css
h2
{
	font-family: times;
	color: #fefefe;
}
```

**ID selecotr**
* apply style only to an HTML tag with the unique identifier we've named
* pretty much all HTML tags can have an `id` attribute
```css
#unique
{
	border: 4px dotted blue;
	text-align: right;
}
```

**class selector**
* apply only to those HTML tags that have been given the same `class` attributes
* much like the `id` attribute, HTML tags can have a `class` attribute
```css
.students
{
	background-color: yellow;
	opacity: 0.7;
}
```
___

# Implementation
There are 3 options for writing stylesheets.

1. Write directly into HTML elements
* Place the stylesheet in an element's `style` attribute
* eg. `<p style="color: green;">hello</p>`

2. Write in the HTML `<style>` tags
* `<style>` tag need to go into `<head>` tag
```html
...
	<head>
		<style>
			p {
				color: green;
			}
		</style>
	</head>
...

```

3. Write in a separate CSS file
* then link it into our HTML, using `<link>` tags
	* `<link>` tag is not [[Common HTML tags#Hyperlink|hyperlink]]
	* it pulls in separate files
		* sort of like [[CS50x 1-2 Programming Basic with C#header file|header file and library]] if it was C, with the `#include<>`
* Perhaps this is the most preferred way
```html
...
	<head>
		<link href="tables.css" rel="stylesheet" />
	</head>
...
```
___

# Comment

```css
td
{
	/* This is a comment in CSS */
	/* // doesn't work though */
	
	background-color: black;
	color: white;
	...
}
```
___
