CSS, shorts for Cascading Style Sheets
* latest version is CSS3
___

# Inline Styling

```html
<h1 style="color: blue; text-align: center;">Welcome to my Web Page!</h1>
```

we give the element a `style` attribute
* we would specify CSS properties in it
	* eg. the color, alignment, ...., that we would like the element to have
	* each property has a default value, but we can change them

Note that HTML element can get their style information from parent elements
```html
<body style="color: blue; text-align: center;">
	<h1>Welcome to my Web Page!</h1>
	Hello, world!
</body>
```
* in this case both parts in the body have the same style
___

# Styling a page

as our web page get more complicated, this coding style quickly becomes a problem
* eg. we have two headings that we would like them to style the same way

surely we can do copy and paste
but later when we want to update the style of the page, we need to change code in two places

so a better design is that, we write the style code once, and apply it to both of the headings

```html
<head>
	<title>Hello!</title>
	<style>
		h1 {
			color: blue;
			text-align: center;
		}
	</style>
</head>
```
___

# Linking CSS

There is again a problem
If we have a web application with multiple web pages, some of them probably are gonna be similar in style
* eg. maybe we want the banner to be the same across the web pages

the solution is, taking the CSS code to a separate file

```css
h1 {
	color: blue;
	text-align: center;
}
```

```html
<head>
	<title>Hello!</title>
	<link rel="stylesheet" href="styles.css">
</head>
```

`rel` for relationship
___

# Common CSS properties


## Size
size of any HTML element

```html
<head>
	<style>
		div {
			background-color: blue;
			width: 100px;
			height: 400px;
		}
	</style>
</head>
<body>
	<div>
		Hello, world!
	</div>
</body>
```

`<div>` for division
* a division of the page, some section of the page, that contains some contents


## Padding and Margin

```html
<style>
	div {
		...
		padding: 20px;
		margin: 20px;
	}
</style>
```

`padding` is on the inside of the border of the element
* by adding padding, `Hello, world!` is not at the very top left corner of the `<div>`

`margin` is on the outside of the border of the element
* by adding margin, `<div>` is not at the very top left corner of the web page


## Font
usually some designer is choosing what font they want for the page

```html
<style>
	div {
		font-family: Arial, sans-serif;
		font-size: 28px;
		font-weight: bold;
	}
</style>
```

we can specify multiple fonts
* because not all fonts are supported by all computers
* in this case, if the computer doesn't support `Arial`, falls back to any `sans-serif` font


## Border

```html
<style>
	div {
		border: 3px solid black;
	}
</style>
```


## eg. CSS on a table

```html
<style>
	table {
		border: 1px solid black;
	}
	
	th {
		border: 1px solid black;
	}
	
	td {
		border: 1px solid black;
	}
</style>
```

That is quite redundent right? So we would normally use a multiple element selector.
```html
<style>
	table, td, th {
		border: 1px solid black;
	}
</style>
```

But, that gives a border for each `td`, `th` element
What we want to do is actually have the borders collapse, so that there is only a single border around all of the cells
```html
<style>
	table {
		border: 1px solid black;
		border-collapse: collapse;
	}
	
	td, th {
		border: 1px solid black;
	}
</style>
```

Lastly, we want the table to have some spacing
* do we want margin or padding? padding.
```html
<style>
	table {
		border: 1px solid black;
		border-collapse: collapse;
	}
	
	td, th {
		border: 1px solid black;
		padding: 5px;
	}
</style>
```
___

# Selectors


## ID selector
id is unique

```html
<head>
	<style>
		#foo {
			color: blue;
		}
	</style>
</head>
<body>
	<h1 id="foo">Heading 1</h1>
	<h1>Heading 2</h1>
	<h1>Heading 3</h1>
</body>
```


## Class selector
class is not unique

```html
<head>
	<style>
		.baz {
			color: blue;
		}
	</style>
</head>
<body>
	<h1 class="baz">Heading 1</h1>
	<h1 class="baz">Heading 2</h1>
	<h1>Heading 3</h1>
</body>
```
___

# Specificity problem

we have a CSS like this? what will happen?
* it actually often times happen
```html
<head>
	<style>
		h1 {
			color: red;
		}
		
		#foo {
			color: blue;
		}
	</style>
</head>
<body>
	<h1 id="foo">Heading 1</h1>
</body>
```

Specificity goes in a particlar order
* there's an order of precedence
* inline > id > class > type
	* basically, it goes how precisely we are specifying an element

so what color Heading 1 is? blue.
* also it doesn't matter if `h1` or `#foo` comes first or last in code
	* because of this specificity rule
___

# More CSS Selectors

| Selectors | name |
| :-: | --- |
| `a, b` | Multiple Element Selector |
| `a b` | Descendant Selector |
| `a > b` | Child Selector |
| `a + b` | Adjacent Sibling Selector |
| `[a=b]` | Attribute Selector |
| `a:b` | Pseudoclass Selector |
| `a::b` | Pseudoelement Selector |


## Descendant Selector
select elements that are descendant of some other element
* descendant = immediate children + grandchildren + grand-grandchildren + .....

```html
<head>
	<style>
		ul li {
			color: blue;
		}
	</style>
</head>
<body>
	<ol>
		<li>list item one</li>
		<li>list item two</li>
		<ul>
			<li>sublist item one</li>
			<li>sublist item two</li>
		</ul>
		<li>list item three</li>
	</ol>
</body>
```

if we typed `ul > li`, it would work the same
* but that's a child selector, meaning it would only select the immediate children


## Attribute Selector

```html
<head>
	<style>
		a {
			color: blue;
		}
		
		a[href="https://facebook.com"] {
			color: red;
		}
	</style>
</head>
<body>
	<ul>
		<li><a href="https://google.com">Google</a></li>
		<li><a href="https://facebook.com">Facebook</a></li>
		<li><a href="https://amazon.com">Amazon</a></li>
	</ul>
</body>
```


## Pseudoclass Selector
style anelement only under certain conditions, or only when an element is in a particular state
* typically done for hovering over something

```html
<style>
	button {
		width: 200px;
		height: 50px;
		font-size: 24px;
		background-color: green;
	}
	
	button:hover {
		background-color: orange;
	}
</style>
<body>
	<button>Click Me</button>
</body>
```
___

# Sass
it's a language that's essentially an extension to CSS
* it adds additional features to CSS
* it's gonna to be faster (to write?), and remove some repetitions

```html
<head>
	<style>
		ul {
			font-size: 14px;
			color: red;
		}
		ol {
			font-size: 18px;
			color: red;
		}
	</style>
</head>
<body>
	<ul>
		<li>unordered item</li>
		<li>unordered item</li>
		<li>unordered item</li>
	</ul>
	<ol>
		<li>ordered item</li>
		<li>ordered item</li>
		<li>ordered item</li>
	</ol>
</body>
```

note in the above code, there is a redundancy
both `ul` and `ol` use the color `red`

one of the key features of Sass is that, it has variables
* in addition, since Sass is a different language to CSS, its file extension is different as well
	* it's `.scss`

```scss
// create a variable, called 'color', assign it a value, 'red'
$color: red;  // so if we want to change the 2 colors, we need only changing it here

// normal CSS syntax
ul {
	font-size: 14px;
	color: $color;  // use the variable
}

ol {
	font-size: 18px;
	color: $color;
}
```

now we try to link the SCSS file in our HTML file
```html
<head>
	<link rel="stylesheet" href="variables.scss">
</head>
```

However, if we open up the web page, the style is not applied
that's because the web browser, like Chrome, Safari, Firefox, can only understand CSS
* by default they don't understand SCSS

The way it goes is to compile the SCSS file, convert and translate it into plain old CSS
* in order to do this, we need to install a program called Sass on our computer
* then in terminal we can run the command `sass variables.scss:variables.css`

Now we have the CSS file, then we can link it, then we have our expected result
___

## automated compilation

when developing our web pages, we're likely to change the style frequently
* if we have to re-compile every darn time, it's gonna be annoying

so `sass` helps us automate the process
* in the terminal, run `sass --watch variables.scss:variables.css`
* now if ever we change the Sass file, `sass` will know about it, and then automatically recompile the corresponding CSS file
* surely, we can do this not only just for a single file, but also the entire directories
___

## nesting CSS selectors

```html
<body>
	<div>
		<p>This is a paragraph inside the div.</p>
		List inside the div:
		<ul>
			<li>item one</li>
			<li>item two</li>
			<li>item three</li>
		</ul>
	</div>
	<p>This is a paragraph outside the div.</p>
	List outside the div:
	<ul>
		<li>item one</li>
		<li>item two</li>
		<li>item three</li>
	</ul>
</body>
```

```scss
div {
	font-size: 18px;
	
	// for paragraphs that are inside of the div
	p {
		color: blue;
	}
	
	// for ul that are inside of the div
	ul {
		color: green;
	}
}
```

so Sass allows a little bit nicer syntax
* then doing `div > p` or `div > ul`

and then here is the compiled CSS version of the SCSS file
```css
div {
	font-size: 18px;
}

div p {
	color: blue;
}

div ul {
	color: green;
}
```
___

## inheritance

```scss
// here we defined a generic message, something that we can extend later
// so later we have the success message, danger message, ..., extended from this
%message {
	font-family: sans-serif;
	font-size: 18px;
	font-weight: bold;
	border: 1px solid black;
	padding: 20px;
	margin: 20px;
}

.success {
	@extend %message;
	background-color: green;  // the additional information, specific to this class
}

.warning {
	@extend %message;
	background-color: orange;
}

.error {
	@extend %message;
	background-color: red;
}
```

and then the CSS version is like this
```css
.success, .warning, .error {
	font-family: sans-serif;
	font-size: 18px;
	font-weight: bold;
	border: 1px solid black;
	padding: 20px;
	margin: 20px;
}

.success {
	background-color: green;
}

.warning {
	background-color: orange;
}

.error {
	background-color: red;
}
```
___
