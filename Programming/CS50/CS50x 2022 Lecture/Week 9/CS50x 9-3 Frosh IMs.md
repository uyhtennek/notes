# eg. Frosh IMs

Now say if we want a web for students to register the intramural sports that they want to play
again we set-up the [[CS50x 9-1 Flask#Flask setup|Flask framework]]

this time our `index.html` is like this
```html
{% extends "layout.html" %}

{% block body %}
    <h1>Register</h1>
    <form action="/register" method="post">
        <input autocomplete="off" autofocus name="name" placeholder="Name" type="text">
        <select name="sport">
            <option disabled selected>Sport</option>
            <option value="Basketball">Basketball</option>
            <option value="Soccer">Soccer</option>
            <option value="Ultimate Frisbee">Ultimate Frisbee</option>
        </select>
        <input type="submit" value="Register">
    </form>
{% endblock %}
```

our `app.py` is still not finished, currently it's like this
```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html")
```
___

# Form Validation

Now there are 2 possible user mistakes that can happen, as they will
1. user inputted an empty `name`
2. user didn't select a sport

since we don't want to store some empty values, or bogus entris, in our database
now let's us do some validation
* and [[CS50x 9-2 Web application#Empty input|previously]] we said we would want that done on the backend
	* that is on the `app.py`

```python
...
@app.route("/register", methods=["POST"])
def register():
	
	# Validate submission
	if not request.form.get("name") or request.form.get("sport") not in ["Basketball", "Soccer", "Ultimate Frisbee"]
		# if there is empty value, that is any of the functions returned "" or none
		return render_template("failure.html")
		
	# Confirm registration
	return render_template("success.html")
```

again, pay some attention to the set-ups:
1. `@app.route("/register", methods=["POST"])`
	* `"/register"` needs to match the `action` of `<form>`, or where the `<form>` is submitted to
	* `methods` need to match `request.form` or `request.args`, or both
2. remember to give the `<input>` an `name` attribute
	* and that `name` we need to match the parameter of the function `request.form.get()`
	* if there is no `name`, there is no key to send to the server, even though there is a value
		* the key value pair is like `="Ultimate Frisbee"`, which doesn't work
		* there is no key, so the browser is not going to send it to the server
	* we can open up Developer Tools and get to the Network tab to see the HTTP request that we sent after submitted the form
		* the information that we inputted is placed at the bottom of the Headers, in the Form Data section

![[register.png]]

> [!tips]
> The point to get to being a good programmer, is not always writting bug-free codes, but is being a good diagnostician.

___

# Jinja's Loops

however, we're repeating the hard-coded sports in both `index.html` and `app.py`
any time there is repeatition, there is an opportunity to optimize, so let's get into it

1. `app.py` is where we want to store the sports, as it's the brain of our web application
```python
...
# firstly we want a "constant" to store the sports
# although Python doesn't really have constant variables
SPORTS = [
	"Basketball",
	"Soccer",
	"Ultimate Frisbee"
]

# we can pass it to the template
@app.route("/")
def index():
	return render_template("index.html", sports=SPORTS)

@app.route("/register", methods=["POST"])
def register():
	
	# Validate submission
	if not request.form.get("name") or request.form.get("sport") not in SPORTS:
		return render_template("failure.html")
	
	return render_template("failure.html")
```

2. we need to change the `<select>` element in `"index.html"`
```html
...
	<form action="/register" method="post">
		<input name="name" placeholder="Name" type="text">
		<select name="sport">
			<option disable selected>Sport</option>
			{% for sport in sports %}
				<option>{{ sport }}</option>
			{% endfor %}
		</select>
	</form>
...
```

this is handy since maybe next year we have a new sport `"Football"`
we just need to add it in the `SPORTS` variable and both the HTML and `app.py` get updated

essentially this is how every website out there does
eg. Gmail
* maybe the inbox is just a big table
* Google Gmail's backend just output `<tr>` after `<tr>` dynamically to the HTML
___

# Other selection options

next, we actually don't want a `<select>` menu for the sports
we want them to be checkboxes so students can select more than just one sport

so we change the `index.html` as such
```html
...
	<form action="/register" method="post">
		<input name="name" placeholder="Name" type="text">
		{% for sport in sports %}
			<input name="sport" type="checkbox" value="{{ sport }}"> {{ sport }}
		{% endfor %}
	</form>
...
```

the result in the actual source code of the html page would be like
```html
	<form action="/register" method="post">
		<input name="name" placeholder="Name" type="text">
			
			<input name="sport" type="checkbox" value="Basketball"> Basketball
			
			<input name="sport" type="checkbox" value="Football"> Football
			
			<input name="sport" type="checkbox" value="Soccer"> Soccer 
			
	</form>
```

notice these elements all have the same name
but that's okay
* it turns out if Flask sees multiple values for the same name, they are handed back to us as a list if we used the right function

okay what if we actually don't want the student able to register multiple sports?
we can change the `type` from `"checkbox"` to `"radio"`
* as radio buttons are mutually exclusive, so we can only sign up for one
* we gave them the same name `"sport"`, that's what makes them mutually exclusive
___

# Store datas

by this point, the so-called frontend is pretty much done
but we still didn't actually register to user's choice to a place

so next let's prepare the `app.py` again
```python
from flask import Flask, redirect, render_template, request
...
# a dictionary, storing key value pairs like
# Carter: "Football", Emma: "Ultimate Frisbee"
REGISTRANTS = {}

SPORTS = {
	"Basketball",
	"Soccer",
	"Ultimate Frisbee"
}
...
@app.route("/register", methods=["POST"])
def register():
	
	# The validation is just the same idea as before
	# the difference is this time it's more expressive
	
	# Validate name
	name = request.form.get("name")
	if not name:
		return render_template("error.html", message="Missing name")
	
	# Validate sport
	sport = request.form.get("sport")
	if not sport:
		return render_template("error.html", message="Missing sport")
	if sport not in SPORTS:
		return render_template("error.html", message="Invalid sport")
	
	# Remember registrant
	REGISTRANTS[name] = sport
	
	# Confirm registration
	return redirect("/registrants")

@app.route("/registrants")
def registrants():
	return render_template("registrants.html", registrants=REGISTRANTS)
```

now if we run the web application, everytime a human register a sport through the website
the information, the key value pair, does get stored in the variable `REGISTRANTS`

and `"registrants.html"` is just a page with a `<table>` to let us preview `REGISTRANTS`
it's like this
```html
{% extends "layout.html" %}

{% block body %}
	<h1>Registrants</h1>
	<table>
		<thead>
			<tr>
				<th>Name</th>
				<th>Sport</th>
			</tr>
		</thead>
		<tbody>
			{% for name in registrants %}
				<tr>
					<td>{{ name }}</td>
					<td>{{ registrants[name] }}</td>
				</tr>
			{% endfor %}
		</tbody>
	</table>
{% endblock %}
```

* also what's inside of those curly braces `{}` and percent signs `%` are essentially Python syntax
___

# implement SQL

but, since `app.py` is running on the computer's memory and our dataset `REGISTRANTS` is stored inside of `app.py`, if we just hit Control + C and kill Flask, all the datas stored would just be gone
* maybe the server reboots, maybe we closed the laptop
* if the server stops running, memory is going to be lost. RAM is volatile.

maybe we should store it in a CSV file
in fact, it is the go-to solution some years ago
but now we have [[CS50x 7-3 SQL#Relational database|SQL and relational database]], which are even better

1. so first thing, let's prepare our database
    let's call it `froshims.db` and it contains just one table and 3 columns, very simple
```sqlite
CREATE TABLE registrants (id INTEGER, name TEXT NOT NULL, sport TEXT NOT NULL, PRIMARY KEY(id));
```

2. once again, let's change our `app.py`
```python
from cs50 import SQL
from flask import Flask, redirect, render_template, request

app = Flask(__name__)

db = SQL("sqlite:///froshims.db")

SPORTS = [
	...
]
...
@app.route("/register", methods=["POST"])
def register():
	
	# Validate submission
	name = request.form.get("name")
	sport = request.form.get("sport")
	if not name or sport not in SPORTS:
		return render_template("failure.html")
	
	# Remember registrant
	db.execute("INSERT INTO registrants (name, sport) VALUES(?, ?)", name, sport)
	
	# Confirm registration
	return redirect("/registrants")

@app.route("/registrants")
def registrants():
	# it gives us back a dictionary
	registrants = db.execute("SELECT * FROM registrants")
	return render_template("registrants.html", registrants=registrants)
```

`db = SQL("sqlite:///froshims.db")`
* `"sqlite:///froshims.db"` is a URI, kinda like if there is a SQLite protocol
	* it just tells Python to locally talk to the file `froshims.db` using sqlite
* `://` is just like what's in URLs,
* the third slash `/` means current folder

`redirect("/registrants)"`
* it does what it said, redirect the user to another route
* it will handle giving back the HTTP `301`, `302`, or `307` code whatever the appropriate one is as a response to the visitor

3. again, `registrants.html` is just a preview
    but anyway let's see how the `registrants.html` looks like
```html
...
	<tbody>
		{% for registrant in registrants %}
			<tr>
				<td>{{ registrant["name"] }}</td>
				<td>{{ registrant["sport"] }}</td>
				<td>
					<form action="/deregister" method="post">
						<input name="id" type="hidden" value="{{ registrant['id'] }}">
						<input type="submit" value="Deregister">
					</form>
				</td>
			</tr>
		{% endfor %}
	</tbody>
...
```

now if we actually register some people
not only we can see the registrants in the preview page `registrants.html`
we can open up `froshim.db` with `sqlite3` and see the registrants are indeed there

4. the last thing is that we implemented a Deregister button there
    but how does it actually works?

the user isn't inputting anything, it's just a button
what does it sends to the server, as a parameter, for the server to delete a registrant?
it's the `"id"`
* again, we can open up Developer Tools and get to the Network tab any time we want to know what are we sending to the server
* like before, this time the Form Data section would be helpful

so let's see where the `"/deregister"` route leads us to
```python
@app.route("/deregister", methods=["POST"])
def deregister():
	
	# Forget registrant
	id = request.form.get("id")
	if id:
		db.execute("DELETE FROM registrants WHERE id = ?", id)
	return redirect("/registrants")
```

but why `id`? not the `name`?
* there could be 2 registrants with the same name

next, why `POST`, not `GET`?
* simply put, it's just not a good design
	* we can do deregistration perhaps too easily and accidentally
* we can easily trick others to click https://www.example.com/deregister?id=3 to deregister themselves
	* turns to a rather big problem if it was a bank account or a Facebook account
	* like accidentally withdrawaled money or automatically post something on Facebook
* it's called a *cross-site request forgery*
	* fancy way of saying tricking people to click a link that they shouldn't have
___

# Send Email

next thing, how about sending an email to the proctor in charge of the intramural sports program so they have a running history of people registered and can easily reply to them as well

let's see the `app.py` for that
```python
# Implements a registration form, confirming registration via email

import os
import re

from flask import Flask, render_template, request
from flask_mail import Mail, Message

app = Flask(__name__)

# Requires that "Less secure app access" be on
# https://support.google.com/accounts/answer/6010255
app.config["MAIL_DEFAULT_SENDER"] = os.environ["MAIL_DEFAULT_SENDER"]
app.config["MAIL_PASSWORD"] = os.environ["MAIL_PASSWORD"]
app.config["MAIL_PORT"] = 587                 # the TCP port to use
app.config["MAIL_SERVER"] = "smtp.gmail.com"  # Gmail's server
app.config["MAIL_USE_TLS"] = True             # use encryption
app.config["MAIL_USERNAME"] = os.environ["MAIL_USERNAME"]
mail = Mail(app)  # created kinda like an email variable
...
```

1. since we used the `flask_mail` library,
    remember to add that to `requirements.txt`, and install email support for Flask first
```
Flask
Flask-Mail
```

2. `app.config`
    it's a special dictionary that comes with Flask, a property of the `Flask` object

3. `os.environ`
* for security purposes, we wouldn't want to hard code our own Gmail username and password
* we can store those in some *environment variables*
	* it's common to see sensitive information are stored in the computer's memory
		* so that it can be accessed when the website is running
	* but not in our source code
		* because others may see our source code
		* maybe we need to post the code to GitHub, or screenshot it accidentally
* `os.environ` is essentially a dictionary, storing the environment variables
	* then the information can still be accessed by computers but never show up in the our code
	* we need to run some command to store some values to `os.environ`

4. be careful that the email may ends up in Spam though
5. we did need to configure the email to enable 'Less secure app access' which used SMTP
	* SMTP is the protocol used for outbound email
6. sure enough, our `index.html` asked user for what email address they want the confirmation email be sent to

but we just saw how the `app` is configured, not how the email is actually sent
so let's us see the rest of `app.py`
```python
...
def register():
	
	# Validate submission
	name = request.form.get("name")
	email = request.form.get("email")
	sport = request.form.get("sport")
	if not name or not email or sport not in SPORTS:
		return render_template("failure.html")
	
	# Send email
	message = Message("You are registered!", recipients=[email])
	mail.send(message)
	
	# Confirm registration
	return render_template("success.html")
```

`Message("You are registered!", recipients=[email])`
* `Message()` is a function from `flask_email` library
* `"You are registered!"` is the subject line
* a named parameter `recipients` takes a list of emails that should get the confirmation email
	* here we just need to send it to one email address

then `mail.send(message)` just sends the `message`, or email
___
