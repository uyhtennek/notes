# Development Trends

HTML, CSS, JavaScript, Python, SQL are the languages for writing web applications,
But it's also increasingly the way that mobile apps are written as well.

* although there are Swift for iOS, and Java for Android
* writting mobile apps in 2 different languages double the work load

* it's not necessarily Python and SQL for the backend though
* but HTML, CSS, and JavaScript are common
___

# Limitation of `http-server`

[[CS50x 8-3 HTML#`http-server`|http-server]] only comes on certain computers
* we can install for free
* written in JavaScript
* we've been using it to run a web server in VS Code
	* but we can run it on our PC or Mac or anywhere else

it can only serve up static content
* HTML files, CSS files, JavaScript files, images, video files, etc.
* all are static content
* It can't really interact with the user beyond simple clicks
	* like if a human submitted a web `<form>`, unless it's actually submitted elsewhere like [google.com](https://google.com), it's not actually going to go anywhere
		* like the last time we submit to `https://www.google.com/search?q=...`
	* because the server can't process the request that is coming in

There is another type of server that can process user input
* comes with Python
* not only just serve web pages
* recall the user inputs are ultimately come from [[CS50x 8-5 URL parameter]]
___

# URL parameter and Server
remember how URL parameter works?

`https://www.example.com/`
* the slash `/` in the end connote the root of the web server
	* like the default folder, where a file called `index.html` or something else is there
* we can also explicitly mention a file or folder or a file in a folder
	* `https://www.example.com/file.html`
	* `https://www.example.com/folder/`
	* `https://www.example.com/folder/file.html`
* typically this part of an URL is called a *path*
	* or *route* is perhaps a more accurate name

because it doesn't have to refer, or map to, a specific folder or a specific file
we just need to make sure that when the user visits a URL with some route, it leads to some where

`https://www.example.com/route?key=value`
* and if we want input from the user, we can add a question mark and a HTTP parameter
	* like google does with https://www.google.com/search?q=cats
* if we have more than just one, we can put one or more ampersand `&`
	* eg. https://www.google.com/search?q=cats&oq=cats
* but this way we can't really do analyze and extract things like `q=cats`
	* it is just appended there but it doesn't actually do anything useful
	* unless we have some fancy JavaScript
* the server itself never bother looking in that for us

instead of using a HTTP server alone, we can use a web server implemented in Python
that can automatically look for any key value pairs after the question mark
* then it'll hand us the pairs in the form of a Python [[Python Syntax#Dictionaries|dictionary]]
* a `dict` object, which is just key value pairs
* this process is automatically handled by a *framework*
	* a framework is essentially a bunch of libraries, and a set of conventions, for doing things
	* eg. [Bootstrap](https://getbootstrap.com) is a framework
		* the downside is that we kinda have to go all in
		* we have to use Bootstraps classes, have to lay out the HTML `<div>`, `<span>`, `<table>` tags in a sort of Bootstrap-friendly way
___

# Flask

Flask is a framework much like Bootstrap
* Bootstrap is used for CSS and JavaScript
* Flask is used for Python
* common problems it solved are
	* easier to analyze the URLs, and get key value pairs
	* easier to find files or images that the human want to see when visiting our website
	* easier to send emails automatically

we need to adhere some requirements though
* `app.py`
	* this is where the web app or web application is going to live
* `requirements.txt`
	* a simple text file contains libraries that we want to use
		* where we list the names of those libraries top to bottom
		* similar in spirit to `#include` in C and `import` in Python
	* the name is a convention
* `static/`
	* contains files that we create that are not ever going to change
	* eg. images, CSS files, JavaScript files
* `templates/`
	* contains HTML that we write
	* web pages that we want the human to see

Do we have to follow these requirements?
No, we don't have to use this framework. But if we do, we must follow the rules.
* we can use other frameworks like Django or asp.net or others, yet they have other different conventions for creating applications
* Flask is a nice microframework, the requirements are pretty minimalist actually
___

# Flask setup

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/")
def index():
	return render_template("index.html")
```

`app = Flask(__name__)`
* recall we have `if __name__ is __main__:` statement
* `__name__` means the name of the current file
* this line say to Python "Turn this file into a Flask application"
	* by calling the `Flask()` function

`render_template("index.html")`
* `render_template()` means render, or print to the user's screen, this file

`@app.route("/")`
* it tells Flask when to call this `index()` function
	* it tells Python to define a route for `"/"`, the default page
	* the function name for `index()` can be anything we want, but here we just make it consistent with what the route is called
* this is known as a *decorator*
	* it's a special type of function, that modifies another function

Next thing we need to create the file `index.html`
and place it under the `templates/` folder

Then how do we start the web server?
* before we would use `http-server` but it's not a Python thing and it has no idea about Flask and its convention stuffs like the `app.py` that we just wrote
	* it just spits out static files
	* `app.py` would not be executed
* this time we use `flask run`
	* at this point `requirements.txt` isn't strictly necessary just yet
	* but surely we need to install the `flask` program first
	* be careful that we must run Flask wherever the directory `app.py` is in
* then we can it gives us a URL
	* and a pop up saying the web app is running on TCP port somthing
	* by default Flask prefers port `5000`
		* although `http-server` prefers `8080`
		* just because
___

# Flask template

the web app would work just fine
but notice the URL would end with nothing or implicitly with slash `/`
even when we put `/?name=David` at the end of the URL the web app still works fine

* the question mark `?` delineates the route and the user inputs
* it's one part of HTTP protocol

Then the question is how can we get the URL parameter in code?
for that let's go back to `app.py`
```python
...
@app.route("/")
def index():
	name = request.args.get("name")
	return render_template("index.html", name=name)
```

`name = request.args.get("name")`
* it asks Flask to
	1. go into the current request, the [[CS50x 8-2 Request and Response#Request and Response|HTTP request]]
	2. go into its arguments
		* that are in the URL
	3. and get whatever the value of the parameter called `"name"`
* finally put that value to the variable `name`

`render_template("index.html", name=name)`
* the function actually can take more than one argument
* the left `name` is the name of the variable we give to the `"index.html"` template
	* so the left `name` need to match the `name` in `"index.html"`
	* we can use something else other than `name` though, if we change it to `name_of_person`, be sure to also change it in `"index.html"`
		* using the same name, that is the left and the right both named `name`, is arguably better
* the right `name` is the variable that we get the value from
	* so if we set this to `"Emma"` the `"index.html"` would always recognize the value of `name` is `"Emma"`, no matter what is the URL parameter

and in `"index.html"`, when ever we want to plug in that variable, we put two curly braces `{{}}`
```html
...
	<body>
		hello, {{ name }}
	</body>
...
```

* that's what it means by *template*
* `"index.html"` is the template of the response for the HTTP request
	* the HTML file after the value of `name` got plugged in to `{{ name }}` is the actual response
* kinda like the [[CS50x 6-1 Python Beginner Tour#`print()`|Fstring]], or format string, in Python
	* this time the syntax hope that we the programmer will never want to display two curly braces `{{}}` in our actual web page
	* but even if we do, there's a workaround

then after we restart `flask run` in terminal so our changes get loaded
when we place `/?name=David` at the end of the URL
it actually says `hello, David` or `hello, Carter` or what ever the name is

also notice we don't need to bother processing the URL, figuring out where is the question mark `?`, the equal sign `=`, the ampersand `&` in the URL
* Flask, the framework, does all of that
* all we need is just calling the function `request.args.get()`
___
