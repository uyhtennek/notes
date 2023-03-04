It's all about making sure that our web pages look good, no matter how users are looking at the web page.
* on computers, on mobile phones, on tablets, ....

And there're a number of different ways that we can implement responsive design
___

# viewport
viewport is the visual part of the screen, that the user can actually see

firstly, how web pages are displayed on mobile devices?
well, one thing that many mobile devices do by default, is treat their viewport as though it is the same width as a computer screen
* because not all web pages are optimized for all mobile devices
* so mobile device makes sure the user can see everything by doing this

as a result, the page is shrink down on mobile devices
![[viewport.png]]

surely this isn't ideal
* becasue everything is too small, and it doesn't look good
* ideally we want our page to adapt to differnt sized screens
![[viewport-responsive.png]]

the first thing that we can start doing is,
add a little line of code to our HTML, inside the `<head>` section, that controls the viewport

`<meta name="viewport" content="width=device-width, initial-scale=1.0">`
* this provides some metadata to our HTML page
* and say, "hey, change the viewport to be the width of the device"

if we don't say that, usually mobile phones would use a viewport that's actually wider than the width of the device, treating it as if it's loading a page in a computer, then shrinking it down to the size of the mobile device

if we specify to use the device width as the viewport width, often times a page looks whole a lot better on a mobile device
___

# Media Queries
with media queries, we can control how our page is going to look, depending on how the page is rendered, or what size is the screen that's rendering the page

basically, if the size of the screen is of certain width, we style the page in one way
and if the size of the screen is of different width, we style the page in a different way
```html
<head>
	<!-- if the width of the page is 600 pixels or larger, display red -->
	<!-- if the width of the page is 599 pixels or smaller, display blue -->
	<style>
		@media (min-width: 600px) {
			body {
				background-color: red;
			}
		}
		@media (max-width: 599px) {
			body {
				background-color: blue;
			}
		}
	</style>
</head>
<body>
	<h1>Welcome to My Web Page!</h1>
</body>
```

there are different media queries as well
* eg. whether a mobile device is vertical or landscape
* eg. whether the user is viewing the page on a computer screen or trying to print the page
___

# Flexbox
a feature from the latest version of CSS

basically, it can turn 1 long row on a computer screen,
to 2 or more short rows on a mobile device's screen
* it wraps the content

```html
<head>
	<style>
		#container {
			display: flex;
			flex-wrap: wrap;
		}
		
		#container > div {
			background-color: springgreen;
			font-size: 20px;
			margin: 20px;
			padding: 20px;
			width: 200px;
		}
	</style>
</head>
<body>
	<div id="container">
		<div>1. This is some sample text inside of a div to demo Flexbox.</div>
		<div>2. This is some sample text inside of a div to demo Flexbox.</div>
		<div>3. This is some sample text inside of a div to demo Flexbox.</div>
		<div>4. This is some sample text inside of a div to demo Flexbox.</div>
		....
	</div>
</body>
```

![flexbox.gif (2400×1826) (harvard.edu)](https://cs50.harvard.edu/web/2020/notes/0/images/flexbox.gif)
___

# Grid

Flexbox is one layout paradigm that exists within CSS,
Grid layout is another one.
* for any time we want to arrange things in a grid, where maybe certain columns need to be certain widths, but others can be a little bit more flexible

```html
<head>
	<style>
		#grid {
			background-color: green;
			display: grid;
			padding: 20px;
			grid-column-gap: 20px;
			grid-row-gap: 10px;
			grid-template-columns: 200px 200px auto;
		}
		
		.grid-item {
			background-color: white;
			font-size: 20px;
			padding: 20px;
			text-align: center;
		}
	</style>
</head>
<body>
	<div id="grid">
		<div class="grid-item">1</div>
		<div class="grid-item">2</div>
		<div class="grid-item">3</div>
		<div class="grid-item">4</div>
		....
	</div>
</body>
```

`grid-column-gap` specifies how much space is between each of the columns
`grid-row-gap` is the same

`grid-template-columns` specifies how many columns there are going to be, and how wide each of those column be
* in our case, first column is `200px` wide, second column is `200px` width, the third column can be what ever the width so just adjust it automatically

![grid.gif (2826×1532) (harvard.edu)](https://cs50.harvard.edu/web/2020/notes/0/images/grid.gif)
___

# Bootstrap

actually, there are libraries that people have already written, that do a lot of the above stuffs
* to make the text look good, to make the buttons look good, to implement responsive design, ....
* one of those, is called Bootstrap
	* it's a popular CSS library that we can use
	* so we don't need to write all the styling from scratch

visit https://getbootstrap.com
* we apply the Bootstrap powered styling by adding classes to the HTML elements


## Column
Bootstrap has its own responsive solution, called *Bootstrap's column model*

Bootstrap divides pages into 12 distinct columns
* each *row* in Bootstrap is divided into 12 columns
```html
<body>
	<div class="container">
		<div class="row">
			<!-- 4 x 3-columns-wide div = 12 columns wide -->
			<div class="col-3">This is a section.</div>
			<div class="col-3">This is another section.</div>
			<div class="col-3">This is a third section.</div>
			<div class="col-3">This is a fourth section.</div>
		</div>
		<div class="row">
			<!-- 3 + 6 + 3 = 12 columns wide -->
			<div class="col-3">This is a section.</div>
			<div class="col-6">This is another section.</div>
			<div class="col-3">This is a third section.</div>
		</div>
	</div>
</body>
```

one of the advantages of using Bootstrap columns is that,
they too can be mobile responsive
```html
<body>
	<div class="container">
		<div class="row">
			<!--  -->
			<div class="col-lg-3 col-sm-6">This is a section.</div>
			<div class="col-lg-3 col-sm-6">This is another section.</div>
			<div class="col-lg-3 col-sm-6">This is a third section.</div>
			<div class="col-lg-3 col-sm-6">This is a fourth section.</div>
		</div>
	</div>
</body>
```

`lg` stands for large screen
`sm` stands for small screen
___
