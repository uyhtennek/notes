# HTML, Python, Flask

It's really common to use Python to write website backends
* Python provides a lot more native functionality to support things like networking, opening socket connections, maintaining connections between two different location on the Internet
* C can do these things too but it's really, really difficult, and really a lot of code
* Python is just a lot more easier to write complex web apps than C

Then there are web frameworks to make things even easier
* since they abstract some Python things away and provide helper functions
* it's like a library that's written in Python and specialized to doing some web things
	* so we don't need to code those ourselves
	* creating a web server in Python is not that many lines of code, but perhaps those frameworks did a better job than us and why not use a tool that already exist
* eg. Django, Pyramid, and Flask
* CS50 uses Flask because it's lightweight while still feature-rich
	* since the amount of space and memory we are allowed to use in CS50 IDE is limited by design, using the lightest weight web framework is important

pure HTML website is static
* if we want a website that displays the current time
* we actually need to update the HTML manually every minute, every second, every millisecond
	* even if we somehow enjoy doing this it's not possible

Python makes things more flexible and allows the web pages to become dynamic
* without requiring any human intervention
* it automates the process of updating the time

and here is how the code works:
```python
from flask import Flask
# datetime and pytz are two module that are native to Python
from datetime import datetime
from pytz import timezone

app = Flask(__name__)

@app.route("/")
def time():
	now = datetime.now(timezone('America/New_York'))
	return "The current date and time in Cambridge is {}".format(now)
```

the code is in something called `application.py`, or `app.py`, specifically

we don't need to update the time and HTML source every minute
it'll work, at least as long as the server computer works
___

# Flask set-up

1. open up a Python file, in general we will call it `application.py`
* but we can call it anything we want

2. the first line of `application.py`, is `from flask import Flask`
* `flask` is a module; `Flask` is a function, within that module.
* actually `Flask` is a class

3. we also need to initialize a Flask application, by saying `app = Flask(__name__)`
* `__name__` is just the name of the file
* basically this is creating a flask application based on whatever file this line of code appears in
	* it creates an app, a Flask application, from the `application.py` file

4. from here we write functions to define the behavior of the applications

here we have 2 functions. their sole purpose is to tell the user which page they are in.
```python
def index():
	return "You are at the index page!"

def sample():
	return "You are on the sample page!"
```

then how do we associate the functions with the site?
5. by applying a *decorator*, which look like this

```python
@app.route("/")
def index():
	return "You are at the index page!"

@app.route("/sample")
def sample():
	return "You are on the sample page!"
```

* if we visit the route that ends with `"/"`, server will run the `index()` function
* if we visit the route that ends with `"/sample"`, server will run the `sample()` function

* decorators associate a particular function with a particular URL
* decorators are actually not native to Flask, they are native to Python
	* but CS50x doesn't cover other uses of decorators
* generally it's used to modify the bahvior of a function, or associate a function with something
	* in Flask specifically we would use it to associate function with visiting certain domains

6. now we can run the web app, but it too requires some steps
* specifically 3 commands to run in terminal or CLI

```bash
export FLASK_APP=application.py
export FLASK_DEBUG=1
flask run
```

* first command we export the `FLASK_APP`
	* it's a system variable, that will get stored in the memory of our IDE
	* so if we ever run an application again, it knows which application to run
* we're bascially saving in memory somewhere, the location of our Flask application
	* by storing it in a system variable

* the second line is optional, but it's recommended
	* `1` just means `true`
* so when we run the Flask application in our IDE, we can see all the things that it's doing
	* so if something go wrong, we could see it printed in terminal

* finally `flask run` just start running the flask application, that is the server
	* this one is more like `http-server`
	* it starts the server and gives us the URL of the application's home page when it's ready

the first 2 commands are just some set-up
* just execute once then it's okay
* later on we just need to execute `flask run` to run the server
* until we want to run another web server, then we need to do the set-up again
___

# Pass Data into Flask application

1. data can be passed in via URLs, akin to using HTTP `GET`
    Flask application also can receive data via URLs

the function would be something like
```python
@app.route("/show/<number>")
def show(number):
	return "You passed in {}".format(number)
```

if users visit some URL ends with `"/show/10"`
it returns `"You passed in 10"`

2. we can pass data via HTML `<form>`
* we can set its submit `method` to either `"get"` or `"post"`
	* recall the [[CS50x 9-2 Web application#POST|difference]] between `"get"` and `"post"` request if you don't know which one to use
* although by default, Flask only accepts HTTP `GET` requests or information via URL
	* we need to configure the decorator a bit to make it works with `POST` request

```python
@app.route("/login", methods=['GET', 'POST'])
def login():
	if not request.form.get("username")
		return apology("must provide username")
	...
```

be ware that `request.form.get()` doesn't mean it's using the `GET` method
* it's just saying go and retrieve from the `form` the `"username"` field

also we can vary the behavior of our function depending on the type of the HTTP request received
* we can do one thing if we got a `GET` request
* do another thing if we got a `POST` request

```python
@app.route("/login", methods=['GET', 'POST'])
def login():
	if request.method == "POST":
		# do one thing
	else:
		# do a different thing
	...
```
___

# Other Flask functions

`url_for()`
* so like the route for the `login()` function in the example is `"/login"` right
* but sometimes it can be a rather long route or long URL
	* for instance `"/user/2022/cs50/intro/login"` or even longer
* if we have some other function that should redirect users to the `"/login"` route
	* we need to type `redirect("/user/2022/cs50/intro/login")`
	* that is not very convenient
* but, since the `login()` function is associated with a decorator, with that very long URL
	* we can instead type `redirect(url_for(login))`
* so it means *URL for some function*

`redirect()`
* just redirect users from one page or one route, to another

`session()`
* it's like a global variable, accessible by all pages
	* not necessarily an HTTP thing, but it's usually there in the headers as well
* we can store data in it
	* maybe for tracking if a user logged in
	* even if they're going to different pages on our site

`render_template()`
* it creates pages on our site
	* usually the pages are mixed with HTML and Python together

**More about Flask**
http://flask.pocoo.org/docs/0.12/quickstart/

**More about Jinja**
http://jinja.pocoo.org/
* Jinja is Python-inspired syntax
___
