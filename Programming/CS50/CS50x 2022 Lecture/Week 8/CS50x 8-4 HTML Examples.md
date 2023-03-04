Here we will look at some of the common HTML tags and attributes to use.
___

# paragraphs.html

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <title>paragraphs</title>
    </head>
    <body>
	    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus convallis
	    scelerisque quam, vel hendrerit lectus viverra eu. Praesent posuere eget
	    lectus ut faucibus.
	    
	    Etiam eu velit laoreet, gravida lorem in, viverra est.
	    Cras ut purus neque. In porttitor non lorem id lobortis. Mauris gravida metus
	    libero, quis maximus dui porta at. Donec lacinia felis consectetur venenatis
	    scelerisque.
	    
	    Nulla eu nisl sollicitudin, varius velit sit amet, vehicula erat.
	    Curabitur sollicitudin felis sit amet orci mattis, a tempus nulla pulvinar.
	    Aliquam erat volutpat.
    </body>
</html>
```

What is wrong here?
If we load the html to a browser, it would be one long massive paragraph, instead of 3.

However browser indeed did exactly what we say
* each of these tags tells the browse, to start doing something and then some lines later stop doing something
* it's like "hey browser, here comes my html"
	* then "here comes my head"
		* and "here comes my title of my page"
			* which is `paragraphs`
		* "hey, that's for my title"
	* "that's for my head"
	* "here comes the body"
	* ....

So if we want a paragraph, we need a `<p>` tag for paragraph
```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <title>paragraphs</title>
    </head>
    <body>
	    <p>
		    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus convallis
		    scelerisque quam, vel hendrerit lectus viverra eu. Praesent posuere eget
		    lectus ut faucibus.
	    </p>
	    <p>
		    Etiam eu velit laoreet, gravida lorem in, viverra est.
		    Cras ut purus neque. In porttitor non lorem id lobortis. Mauris gravida
		    metus libero, quis maximus dui porta at. Donec lacinia felis consectetur
		    venenatis scelerisque.
	    </p>
	    <p>
		    Nulla eu nisl sollicitudin, varius velit sit amet, vehicula erat.
		    Curabitur sollicitudin felis sit amet orci mattis, a tempus nulla pulvinar.
		    Aliquam erat volutpat.
	    </p>
    </body>
</html>
```
___

# headings.html

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <title>headings</title>
    </head>
    <body>
		<h1>One</h1>
		<p>
			paragraph one
		</p>
		<h2>Two</h2>
		<p>
			paragraph two
		</p>
		<h3>Three</h3>
		<p>
			paragraph three
		</p>
    </body>
</html>
```

`<h#>` tag gives us headings
* `<h1>` is the biggest
* `<h2>` is the second biggest
* `<h3>`, the third biggest
* all the way down to `<h6>`
	* after that, maybe we should re-organize our thoughts
___

# list.html

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <title>list</title>
    </head>
    <body>
	    <ul>
		    <li>foo</li>
		    <li>bar</li>
		    <li>baz</li>
	    </ul>
    </body>
</html>
```

`<ul>` stands for *unordered list*
`<li>` stands for *list item*

together they form a list of items, looking like this
* foo
* bar
* baz

now what if we don't want it to be a default bulleted list,
what if we want this list to be numbered?
* instead we use `<ol>` for *ordered list*

1. foo
2. bar
3. baz

notice we just need to change `<ul>` to `<ol>` and don't need to touch any the `<li>`
which is a nice thing to have
* because otherwise we would have to re-order every item if we just add or delete item from the start or the middle
* instead the computer is doing it for us by just numbering from top to bottom
___

# table.html

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <title>table</title>
    </head>
    <body>
		<table>
			<thead>
				<tr>
					<th>Name</th>
					<th>Number</th>
				</tr>
			</thead>
			<tbody>
				<tr>
					<td>Carter</td>
					<td>+1-617-495-1000</td>
				</tr>
				<tr>
					<td>David</td>
					<td>+1-949-498-2750</td>
				</tr>
			</tbody>
		</table>
    </body>
</html>
```

`<table>`
* where the table starts

`<thead>`
* table head
`<tbody>`
* table body

`<tr>`
* table row
`<th>`
* table heading
`<td>`
* table data
* or *cell* if it would be in Excel or Google Spreadsheet

Although the table is not gonna be pretty
* but with HTML alone we're just focusing on the structure alone
* to make it pretty, it's something else's job
	* even as minor as just to indent things, it's better to use CSS
___

# image.html

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <title>image</title>
    </head>
    <body>
		<img src="harvard.jpg">
    </body>
</html>
```

Image actually doesn't have an end tag
* it's kinda illogical, since how can we start an image and then eventually finish it?
* an image is either there or isn't
	* so some tags actually don't have end tag

`<img src="harvard.jpg">`
* `src` stands for source
* `"harvard.jpg"` is the image filename for sure
	* and the file has to be in the same folder as the `image.html`
		* so saying `<img src="./harvard.jpg">` is the same thing
	* if it's an image from the Internet we can do
	   `<img src="https://www.harvard.edu/folder/harvard.jpg">`

> [!tip]
> one other thing, we can upload files to the CS50 VS Code environment by simply dragging and dropping the files into the CS50 VS Code's directory

* for accessibility purposes, for someone who's vision-impaired, it's ideal if we also give the image tag an alternative text
	* `<img alt="Harvard University" src="harvard.jpg">`
	* it's called the *alt tag*
* so that screen readers will recite what it is the photo is for people who can't see it
* also sometimes if we're on a slow connection, we can see the alt text before the image finished download, especially on a mobile device
	* so that we can know what we're about to see

* the size or the layout of the image is probably very wrong
* but again, fixing it is not HTML's job, but CSS's
* although there are some historical attributes that we still can use
	* to control width, height, and so forth
	* CSS is better for the job, as it's the language designed for just that
___

# video.html

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <title>video</title>
    </head>
    <body>
	    <video autoplay loop muted width="1280">
		    <source src="halloween.mp4" type="video/mp4">
	    </video>
    </body>
</html>
```

`<source src="halloween.mp4" type="video/mp4">`
* `src`, again, the source file
* `type`, content type, or MIME type, of the file

`<video>` tag actually has a few attributes
* `autoplay` just auto-play
* `loop`, loop forever
* `muted`, so that there's no sound
	* which is in fact necessary nowadays
	* Most browsers, to prevent ads, don't autoplay videos, if they have sound
		* if we need `autoplay` we need `muted`
	* so it would not annoy users
* `width` can be any size

notice one thing, sometimes attributes don't have values
they are *empty attributes*, just a single words
* because it wouldn't make sense if we had things like `muted=0` or `muted=1`
* because it's either muted or not, the attribute is there or not
	* if it's not muted, we simply just don't add this attribute
___

# iFrame.html

if you have your own YouTube account, or your own blog or WordPress site or Wix or Squarespace
you might have been in the habit of embedding videos in websites, using like embedded YouTube players

it's possible too in HTML, using what's called an *inline frame*, an *iFrame*
it's again is just a tag
```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <title>iframe</title>
    </head>
    <body>
		<iframe allowfullscreen src="https://www.youtube.com/embed/xcFZjo5PgG0">
		</iframe>
    </body>
</html>
```

`<iframe allowfullscreen src="https://www.youtube.com/embed/xcFZjo5PgG0">`
* `src`
	* followed by a URL
	* if it happens to be a embedded YouTube players, there is a certain URL format that we need to follow, per YouTube's documentation
	* it's how CS50 embed their own lecture videos in the course's website
		* the video player does exactly this
* `allowfullscreen`
	* maybe need to check the documentation for more available attributes like this one

`<iframe>` is a way to embed someone else's web page in our web page
* if they allow it, fair enough
* so there could be many fun stuffs happening here
___

# link.html
web page that actually links from itself to somewhere else

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <title>link</title>
    </head>
    <body>
		Visit <a href="https://www.harvard.edu/">harvard.edu</a>.
    </body>
</html>
```

Basically in any websites nowadays, if we just type in a domain name, or a fullly qualified domain name, it automatically becomes a link
* That's because those websites have code in them that automatically detect something that looks like a URL and turns it into a proper link

HTML itself does not do that
* So if we type `Visit harvard.edu`, the `harvard.edu` would not be a link just automatically
* we need the `<a>` tag

`<a href="https://www.harvard.edu/">`
* `a` for *anchor*
* `href` for *hyper-reference*
	* perhaps it's eaiser to think of it as *hyper-link*

* we can make the text that the human see, still is `harvard.edu`, by encapsulate it with the `<a>` tag
* but in fact where the link leads him or her to, is `https://www.harvard.edu/`
	* and even we actually leads the user to `https://www.yale.edu/`, but the text is show up is still going to be `harvard.edu`
	* this is called a *phishing attack*
		* it's this easy to deceive people. simply by just saying one thing and linking to another.

When we go into the web page, and hover the mouse over the text, but don't click it,
it will show, in most browsers, a little clue as to where we will go if we actually click on the link
* so that we can see beforehand is the URL that links to correct or not

Lastly we can do not only linking to websites from the Internet
it also allow linking to another page or file that is in the same directory or same machine
* `<a href="image.html">harvard.edu</a>`
	* the `image.html` part will get appended automatically to the current fully quailitied domain name that the user at
* and this is basically how to create a multi-page website
___

# responsive.html

*Responsive* means, in the context of web, responding to the size of the user's device
* it's important because users can use whatever device, phone or PC, whose screen size can be as small as a phone screen or as big as a TV screen, to browse our pages

There are special tags that we can use to tell the browser to modify its display, based on the hardware
But before introducing the tag itself

Here's a trick using Chrome or Edge or other common browsers, that we can pretend to be another device, so that we later can test if our html works out
* Go to View > Developer > Deverloper Tools
* There is an icon looks like mobile devices
* And there is an option saying **Dimensions**
	* which allow us to simulate any phone model and screen size
![[Chrome-simulate-phone.png]]

But notice that the text size would be the same no matter the screen size, if we don't have the special responsive tag
* meaning the text on phone typically would be quite small

So let's add the tag
```html
<!DOCTYPE html>

<html lang="en">
    <head>
	    <meta name="viewport" content="initial-scale=1, width=device-width">
        <title>responsive</title>
    </head>
    <body>
		Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus convallis
	    scelerisque quam, vel hendrerit lectus viverra eu. Praesent posuere eget
	    lectus ut faucibus.
	    
	    Etiam eu velit laoreet, gravida lorem in, viverra est.
	    Cras ut purus neque. In porttitor non lorem id lobortis. Mauris gravida metus
	    libero, quis maximus dui porta at. Donec lacinia felis consectetur venenatis
	    scelerisque.
	    
	    Nulla eu nisl sollicitudin, varius velit sit amet, vehicula erat.
	    Curabitur sollicitudin felis sit amet orci mattis, a tempus nulla pulvinar.
	    Aliquam erat volutpat.
    </body>
</html>
```

`<meta name="viewport">`
* `meta` tag
	* allows us to specify the name of some kind of configuration detail, or property
* `"viewport"` is the technical term for the rectangular region that the human sees in a browser
	* essentially the `<body>` of the page
	* but only the part that is currently showing
* `"initial-scale=1, width=device-width"`
	* it's just some magical statements that we just need to know that it does something, and then we just copy and paste from some where
	* in this case this statement express to the browser, assume that the width of the page is the same thing as the width of the device
		* don't assume the screen size is big enough or wide enough
___

# Others

`Q:` Do attributes have order?
`A:` No. It can be in any order.
	Although, if you do want it to be a little bit more neat, you can alphabetize them.
	So that any missing attributes can be easily spotted.
	Just most people on the Internet don't seem to do that.

HTML is starting to replace other languages for user interfaces.
* Actually it's not just HTML alone, but together with CSS and JavaScript.
* This has been the trend for portability
	* so companies, or individual programmers, can just write one version of an app and have it work on Android devices and iPhones and Macs and PCs, and the like
* Since it's expensive and time-consuming to
	* learn a language like Java to write an Android app
	* then yet learn another language, Swift specifically, to make an iOS app
	* Not to mention to make them look and behave the same
	* Not to mention fix a bug in one and then remember to fix the other
___
