Now say we're making our home page, `home.html`
Right now it's just 3 messages in 3 paragraphs as such.
```html
<!DOCTYPE html>

<html lang="en">
	<head>
		<title>home</title>
	</head>
	<body>
		<p>
			John Harvard
		</p>
		<p>
			Welcome to my home page!
		</p>
		<p>
			Copyright (c) John Harvard
		</p>
	</body>
</html>
```

It looks awful...
___

# `<div>`

First thing, `<p>` is not quite the right tag to use here.
* The 3 messages are not really paragraphs.
* The should be placed in different areas of the page, or divisions of the page
	* they are not meant to be put next to each other
	* it should be something, `John Harvard` is the header so it should be placed at the top of the screen or page, `Welcome to my home page!` is the main part and there is that copyright footer

* `<div>`, for *division*, is a more proper choice here
	* it's a very commonly used tag in HTML
	* which just gives a generic, invisible rectangular region
		* but it doesn't do anything else
	* inside of which we can start to style things

```html
<!DOCTYPE html>

<html lang="en">
	<head>
		<title>home</title>
	</head>
	<body>
		<div>
			John Harvard
		</div>
		<div>
			Welcome to my home page!
		</div>
		<div>
			Copyright (c) John Harvard
		</div>
	</body>
</html>
```
___

# semantic tags

Also there are tags, that have names literally describe what they are
* That's how they got their name, *semantic* ( 語義上的 )
* but aesthetically they look the same as `<div>` or technically they have no different than `<div>` other than they don't use a generic name like `div`
Among them there could be some even more proper choices than just `<div>`

```html
<!DOCTYPE html>

<html lang="en">
	<head>
		<title>home</title>
	</head>
	<body>
		<header>
			John Harvard
		</header>
		<main>
			Welcome to my home page!
		</main>
		<footer>
			Copyright (c) John Harvard
		</footer>
	</body>
</html>
```

* they're more compelling these days
	* for accessibility, for screen readers, for search engines, etc.
* because it helps screen readers or search engines where to look at
	* they probably don't need to read the `<footer>` right?
	* `<header>` may have something interesting, and `<main>` is probably the most juicy part
___

# `style` attribute

```html
<!DOCTYPE html>

<html lang="en">
	<head>
		<title>home</title>
	</head>
	<body>
		<header style="font-size: large; text-align: center;">
			John Harvard
		</header>
		<main style="font-size: medium; text-align: center;">
			Welcome to my home page!
		</main>
		<footer style="font-size: small; text-align: center;">
			Copyright (c) John Harvard
		</footer>
	</body>
</html>
```

First thing, the value in the `style` attribute, is in fact, **CSS**, *Cascading Style Sheets*
* notice, it too, follows a key-value pattern
* colon `:` separating the key and value, and the semi-colon `;` separating each key-value pair
	* so strictly speaking the semi-colon `;` at the end isn't necessary
	* add it or not to your like. it doesn't matter.

But this is actually quite a weird example
because it's not just CSS, but CSS inside of JavaScript
* as of now, we can use CSS inside of the quote marks `""` in the value of a `style` attribute
* recall that we also did something similar with [[CS50x 7-4 Table Relationships#Python and SQL|Python and SQL]]
* So languages can kind of cross barriers

but sure enough we're not going to do this ultimately
because it's not quite scalable
___

# HTML entity

one little refinement before we move on
notice we didn't use the copyright symbol, but `(c)` to sort of mimic that?

it turns out this is not necessary, there is a way to express that symbol
* although there is no key on our keyboard for this symbol
* we type `Copyright &#169; John Harvard` there in place of `(c)`

`&#169;`
* it's called *HTML entity*
* it turns out there are numeric codes, with this weird syntax
* that would allow us to specify symbols that exist in Macs nad PCs and phones, but not exist on our keyboards
___

# Inheritance in CSS

Also notice we typed `text-align: center` 3 times, for 3 different elements
* [[CS50x 0-2 Brief View of Programming#Repeatation is bad|Repeatation]] is a bad thing, remember?
* Luckily the design of CSS helps us avoid repeatation
	* as it supports *inheritance*
	* whereby children inherit the properties, the key value pairs, of their parents or ancestors

Therefore, we just need to type it once in the parent element, the `<body>`
So that is sort of cascades down to the children, the `<header>`, `<main>`, `<footer>` tags
```html
<!DOCTYPE html>

<html lang="en">
	<head>
		<title>home</title>
	</head>
	<body style="text-align: center;">
		<header style="font-size: large;">
			John Harvard
		</header>
		<main style="font-size: medium;">
			Welcome to my home page!
		</main>
		<footer style="font-size: small;">
			Copyright &#169; John Harvard
		</footer>
	</body>
</html>
```
___

# Proper implementation of CSS

And that's about it, that's as good as we can get with HTML.
Still, it's considered as bad practice
* Everything is just in line with the HTML
* It co-mingles our HTML and CSS
	* then it's hard to separate or distribute works since people would need to edit the same file and worse the same line of code

Instead, we would like a person working with the html file, and then the designer working with the CSS file
* much like we can move stuff into [[CS50x 1-2 Programming Basic with C#header file|header files]] in C, or packages in Python
* we can do the same in CSS

So now, let's remove all the `style` attributes
Instead we'll start using a `<style>` tag
* it maybe confusing since the tag and attribute have the same name `style`, but don't worry it's not very common, and hopefully there is only this one

```html
<!DOCTYPE html>

<html lang="en">
	<head>
		<style>
		
			header
			{
				font-size: large;
				text-align: center;
			}
			
			main
			{
				font-size: medium;
				text-align: center;
			}
			
			footer
			{
				font-size: small;
				text-align: center;
			}
		
		</style>
		<title>home</title>
	</head>
	<body>
		<header>
			John Harvard
		</header>
		<main>
			Welcome to my home page!
		</main>
		<footer>
			Copyright &#169; John Harvard
		</footer>
	</body>
</html>
```

* notice there is a slightly different syntax for expressing the same key value pairs
* In the `<body>`, we only need to care about the structure
	* the `<style>` tag in `<head>` takes care all the `style` properties for us
	* later when the browser encounter the `<header>` tag, `<main>` tag, `<footer>` tag, it knows, because of the `<style>` tag, that it should apply the `style` properties

But again, the `text-align` property repeated! Let's fix it first.
```html
...
		<style>
			
			body
			{
				text-align: center;
			}
			
			header
			{
				font-size: large;
			}
			
			main
			{
				font-size: medium;
			}
			
			footer
			{
				font-size: small;
			}
		
		</style>
...
```

In addition, what we're adding in the `<style>` tag, are called *type selector*.
* We selected types, namely tags, and define their properties.
* As the name implies, there are other types of selector.

> [!Types of selector]
> type selector
> class selector
> ID selector
> attribute selector
> etc.

^3b761b

___

# CSS classes

Let say we finished our home page. Next we may want to create other pages.
In which we might also have centered text, also have large, medium, small text, on other pages.

Wouldn't it be nice if we can reuse these properties again and again?
* Kinda like if we would have our own library.
But right now they only live in the home page.

Therefore, what we could do to reuse the properties, is creating classes.
* Although it's called class, it have nothing to do with or similar to Python's [[Python Syntax#Objects|class]]
* in here class is just a feature

```html
<!DOCTYPE html>

<html lang="en">
	<head>
		<style>
			
			.centered
			{
				text-align: center;
			}
			
			.large
			{
				font-size: large;
			}
			
			.medium
			{
				font-size: medium;
			}
			
			.small
			{
				font-size: small;
			}
		
		</style>
		<title>home</title>
	</head>
	<body class="centered">
		<header class="large">
			John Harvard
		</header>
		<main class="medium">
			Welcome to my home page!
		</main>
		<footer class="small">
			Copyright &#169; John Harvard
		</footer>
	</body>
</html>
```

`.centered` / `.large` / `.medium` / `.small`
* they creates new classes
* after that, those classes can be used in this file or in other web pages
	* but we need to set the `class` attribute of the elements
	* `<body class="centered">`, `<header class="large">`, etc.
* in addition they are called *class selectors*
	* they're selecting all of the tags that have that particular classes and applying the properties
___

# Linking CSS

But, if we keep doing this, adding more classes and more attributes in the `<style>` tag and as the same time more and more elements and more layer of structure in the `<body>`
This HTML file could get very very long, to a point that it's painful to look at.

Therefore it maybe a good idea to link this HTML file, which only cares about the structure, to a separate CSS file, which only cares about the style.

```css
.centered
{
	text-align: center;
}

.large
{
	font-size: large;
}

.medium
{
	font-size: medium;
}

.small
{
	font-size: small;
}
```

```html
<!DOCTYPE html>

<html lang="en">
	<head>
		<link href="home.css" rel="stylesheet">
		<title>home</title>
	</head>
	<body class="centered">
		<header class="large">
			John Harvard
		</header>
		<main class="medium">
			Welcome to my home page!
		</main>
		<footer class="small">
			Copyright &#169; John Harvard
		</footer>
	</body>
</html>
```

the only new thing here is the `<link href="home.css" rel="stylesheet">`

`<link>` tag
* obviously it links the CSS file to this HTML file
* it's weird and misleading that this `<link>` tag is not the one for [[CS50x 8-4 HTML Examples#link.html|hyper-link]]
	* instead it's the `<a>` tag
	* `<link>` for linking files, `<a>` for anchor
	* also `<link>` can only be used in `<head>`

`href="home.css"`
* again, `href` for *hyper-reference*

`rel="stylesheet"`
* `rel` stands for *relationship*
* basically just mean the file type is `"stylesheet"`
* `"stylesheet"` is a file containing a whole bunch of stylizations, or properties

In addition, this design or this approach allows us to
* package our CSS files and give it to others
* use other CSS files instead of re-inventing the same functionality which some body in the world has already did
* actually perhaps this is the best design when it comes to CSS
	* so don't ever use `style` attribute again
___

# Developement Tools

Before we talked about the [[CS50x 8-2 Request and Response#Visiting `harvard.edu`|Network tab]] and the [[CS50x 8-4 HTML Examples#responsive.html|mobile button]] in the developer tool
This time we will look at the Element tab.

![[Chrome-Elements.png|600]]

What's nice about it is that we can see a pretty printed version of the web page's HTML
* we can look at and learn from the HTML source code of any web page on the Internet
* in fact let's learn about this one


## ID selector

In this photo we can see the first paragraph, the first `<p>`, is given an `id` property, with the value being `"first"`
* As a web designer, we can give any tag an `id` attribute in a page
	* a *unique identifier*
	* the onus is on us to make sure the `id` doesn't repeat
		* if we do, it's incorrect behavior
* Only the first paragraph has an `id`, but not in any other paragraphs

In the `<head>` of the page, the `<style>` tag has in fact an ID selector
* ID is represented by a hash `#`, and [[CS50x 8-6 CSS#CSS classes|class]] is represented by a dot `.`
* it tells the browser what ever element has the `first` ID, apply the properties


Now back to the Developer Tools
Okay now so if we want to also make the first paragraph green, we just need to do
```html
...
		<style>
			#first
			{
				color: green;
				font-size: larger;
			}
		</style>
...
```
or `color: #00ff00` if we prefer [[CS50x 4-1 Memory#Color|hexadecimal]] color code

And then we would save the file, then go back to the web page, wait for it to reload
Okay it works fine, but maybe the color don't fit the page, we want to change it red instead.
So we go over the process again, change the file, save the file, reload the page.

This actually get pretty tedious quickly
* it's just too many steps to deal with a small change

So let's bring back our developer tool again.
![[Chrome-Elements.png|600]]

Notice
1. when we move the cursor to highlight over some elements, under the Elements tab, the page's corresponding region or thing will also get highlighted
	* it's showing what the tag, that we highlighted over, represents
2. On the right, we can also see all of the stylizations of the highlighted particular element
	* the `#first { }` one is our thing
	* for the italicized `p { }` element, its stylization is actually built-in
		* it doesn't get shown in the image but there is a line saying *`user agent stylesheet`* in that block
		* meaning it's how Chrome makes paragraphs look like by default, or by user's settings

And we actually can change the stylization in `#first { }`
* we can add properties, change properties, delete properties, etc.
	* actually perhap just turning things on and off is better than deleting
* and the change is real-time
* in addition it's purely client side
	* nothing in our code, HTML file or CSS file get touched
	* when the client receive the web page, the HTML file and other stuffs, as the [[CS50x 8-2 Request and Response#Visiting `harvard.edu`|response]]
	* the files are just a copy
* therefore we can change it how ever we want

3. Actually we can right click on any thing in the web page, and then click Inspect
    Then it will jump us to the element under the Elements tab

![[Chrome-Inspect.png|550]]

* Then we can even delete elements in the web page
	* simply by hitting the Delete key
	* useful for removing ads indeed...
* Since we're just changing our own local copy, if we reload the page, the deleted elements would be there again

**Recap**
Developer Tool is powerful to
1. iterate quickly, try different things, different designs quickly
2. learn how others did something
___

# Pseudo selector

back to the [[#ID selector|first paragraph example]]
perhaps we really don't like the idea of giving the first `<p>` an `id` property

* just like C and Python, there are many ways to handle the same problem
* we can actually don't give any attribute to the first `<p>` but still we can change its style

```html
...
		<style>
			p:first-child
			{
				font-size: larger;
			}
		</style>
...
```

`p:first-child`
* this specify the `p`, paragraph, that happens to be the first child
* similarly we have `last-child`

let's look at another one
```html
...
		<style>
			a
			{
				color: #ff0000;
				text-decoration: none;
			}
			
			a:hover
			{
				text-decoration: underline;
			}
		</style>
...
```

remember what a [[CS50x 8-4 HTML Examples#link.html|<a>]] tag is?
by default the text is underlined, but here we removed the default underline by using `text-decoration: none;` and changed its color to red

`a:hover`
* here we set the style when our mouse hover over the `<a>` element
* in this case it just brings back the underline
___

# attribute selector

lastly we can also do select by attribute
```html
...
		<style>
			a[href="https://www.harvard.edu/"]
			{
				color: #ff0000;
			}
			
			a[href="https://www.yale.edu/"]
			{
				color: #0000ff;
			}
		</style>
...
```

`a[href="https://www.harvard.edu/"]`
* it specifies the anchor tag, who's `href` value happens to be the `"<url>"`, then apply the stylization
* but, if the `href` in the actual `<a>` tag in `<body>` is something like `"https://www.harvard.edu/info/"`
	* the stylization would not be applied, since the `href` is different already
	* so maybe this is a little too precise

To make it more forgiving, we have `a[href*="harvard.edu"]`
* `*=` means contain, `=` rather than means equal
* so this apply to any anchor tag who's `href` contains anywhere in it, `"harvard.edu"`
___

# Framework - Bootstrap

as there are many ways in HTML and CSS to achieve the same result,
it'd be easier to read and develop web if every body follow some guidelines or some conventions
* eg. a company might say, always use classes, don't use IDs, or always use attribute selectors
* it's like every one or every group have their own style guide of sorts

However, in the real world people don't come up with all of their own CSS properties
We would start with something off the shelf, a framework, or a third-party library
* typically a free and open source framework
* that just comes with a lot of pretty stylizations
* eg. [Bootstrap](https://getbootstrap.com/)
	* CS50 uses on all of its websites
	* super-popular

For example, Bootstrap invented classes name `"alert"` and `"alert-warning"`, and already gave the classes some pretty warning alert looks
If we want to use that we just need to link their CSS to our HTML and then set our element a few things, which typically [Bootstrap's documentation](https://getbootstrap.com/docs/5.2/getting-started/introduction/) would guide us how to

```html
	<body>
		<div class="alert alert-warning" role="alert">
			A simple warning alert-check it out!
		</div>
	</body>
```

`class="alert alert-warning"`
* here is actually 2 classes, `alert` and `alert-warning`

`role="alert"`
* it's just makes clear to like a screen reader that this is an alert


In addition, according to its documentation
[`"https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css"`](https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css)
is where its CSS file is stored.
* it's crazy long and has no white space
	* because it helps minimize the file size, since it didn't waste space
* no comments
	* since no need to, they got the documentation

Notice, even though we didn't use any class from Boostrap's CSS file,
just by linking it, the Chrome's default styles are no longer used
* all the `<h1>`, `<p>`, etc. elements look different already
* instead Boostrap's default styles are used
* Which is a way of enforcing similarity across Chrome, Edge, Firefox, Safari, and others


Now how about let's improve our [[CS50x 8-5 URL parameter#Implementing a search feature|search.html]]
```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/
			        bootstrap.min.css"
	          rel="stylesheet"
	          integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftj
			       DbrCEXSU1oBoqyl2QvZ6jIW3"
			  crossorigin="anonymous">
	    <title>search</title>
    </head>
    <body>
        <div class="container-fluid">

            <ul class="m-3 nav">
                <li class="nav-item">
                    <a class="nav-link text-dark" href="https://about.google/">
                    About
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link text-dark" href="https://store.google.com/">
                    Store
                    </a>
                </li>
                <li class="nav-item ms-auto">
                    <a class="nav-link text-dark" href="https://www.google.com/gmail/">
                    Gmail
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link text-dark" href="https://www.google.com/imghp">
                    Images
                    </a>
                </li>
                <li class="nav-item">
                    <a class="btn btn-primary"
                       href="https://accounts.google.com/ServiceLogin" role="button">
                    Sign in
                    </a>
                </li>
            </ul>

            <div class="text-center">
              <img alt="Happy Cat" class="img-fluid w-25" src="cat.gif">
              <form action="https://www.google.com/search" class="mt-4" method="get">
                  <input autocomplete="off" autofocus
	                     class="form-control form-control-lg mb-4 mx-auto w-50"
	                     name="q" placeholder="Query" type="search">
                  <button class="btn btn-light" type="submit">
                  Google Search
                  </button>
                  <button class="btn btn-light" name="btnI" type="submit">
                  I'm Feeling Lucky
                  </button>
              </form>
 
          </div>
        </div>
          
    </body>
</html>
```

`<div class="container-fluid">`
* `"container-fluid"` is a class that comes from Bootstrap
* that can make a web page *fluid*
	* that is grow to fill the window
	* so that way it's going to resize nicely

`"nav"`, `"nav-item"`, `"nav-link"`
* `"nav"` is used for list, `<ul>` tag
* `"nav-link"` is used for an `<a>` tag in a list
* together, we don't understand how, but they creates a pretty list
> [!quote]
> [Bootstrap](https://getbootstrap.com/docs/5.2/components/navs-tabs/) told me to.
* `<ul class="m-3 nav">`
	* `"m-3"` stands for *margin by 3*
	* there are `"m-1"`, `"m-2"`, ..., up to `"m-5"`
	* it adds some amount of padding

`"ms-auto"`
* stands for *margin start - auto*
* in `<li class="nav-item ms-auto">`
* it shove the previous list items and the list items from this one and so on, apart
	* previous list items are placed at the left side of the screen
	* list items from this and so on are placed at the right side of the screen

`<a class="btn btn-primary" ...>`
* `"btn"` turns a link to a button
* `"btn-primary"` makes it blue

In the end this HTML created a Google clone.
* without us touching any raw CSS


But if we want to make refinements, maybe we don't really like the shade of blue that Bootstrap chose, maybe we want to curve things a bit more
That where we can create our own CSS file and do the last mile, fine tuning things
* this tends to be best practice
* standing on the shoulders of others as much as we can, using libraries, until a point that the libraries can't offer us what we're looking for
* then use our own skills and understanding of HTML and CSS to refine things
___

