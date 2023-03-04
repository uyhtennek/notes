# Session

a session is the technical term for what you and I know as a shopping cart
* when we go to https://amazon.com and we start adding things to the shopping cart
* the things, the cart, follow us page to page to page
	* even if we closed the browser and came back the next day, they were still there
	* great for both users and Amazon
* similarly, when we log into any website these days, with username and password reglectless if it was an e-commerce thing or not, we don't need to log in again for quite a while
	* maybe for the next hour, day, week, year, we stay logged into that website

* somehow the website remembers what we have done
	* maybe it's adding something to the shopping cart, or logging in the website
* it's implemented by the thing called *session*
	* perhaps a more familiar term is *cookies*

> [!definition]
> State refers to information. Something being stateful means it remembers information.

the curiosity is that HTTP is technically a stateless protocol
* once we visit a URL, web page is downloaded to the browser, and that's it
* all everything is done locally, the server doesn't care about who we are
	* we can unplug from the Internet, turn off the Wi-Fi, the web page is still there, locally

somehow we want the server to remember who we are or what we did,
the next time we visit the website
* somehow we want to make HTTP stateful
* eg. we probably don't often log in Google manually
	* Google gave us a rather long session time, maybe a month, a year, or some years
* eg. by contrast some of the CS50 zones do need us to log in every time
	* to make sure it's us who accessing the website instead of our roommate or friend or else
___

# Cookie

when we visit a web page, the HTTP request header may looks like this
```
GET / HTTP/1.1
Host: gmail.com
```

and the server responds, perhaps something like this
* a website that is stateful, maybe `gmail.com`
```
HTTP/1.1 200 OK
Content-Type: text
Set-Cookie: session=value
```

It's very commonly the case, websites put a cookie on our computer
* that's both a blessing and a curse
* since cookies track us in some way, some good for user experience, some bad for privacy
	* the bads maybe tracking us on every website and serving us ads more effectively or boringly

A cookie is typically a big, seemingly random, number
* the server tells our browser to store in memory or even disk, this big value
* think of it like a file that a server is planting on our computer

then a part of the HTTP protocol is that
if a server sets a cookie on our computer, we will represent that same cookie every time we send a request to the server

therefore the next time we visit https://gmail.com
we would have a cookie in the request header
* placed in the opposite header of `Set-Cookie`, `Cookie`
* their `value` are the same
```
GET / HTTP/1.1
Host:gmail.com
Cookie: session=value
```

similar to going to a club or an amusement park
where we pay once, go through the gates once, checked by security once,
* very often they would give us a little stamp
* then from now on we are allowed to come and go
	* if we come back later in the day, in the evening, we can just present the stamp
	* then we don't need to pay again, be searched by security again, and just go back in

* That's essentially what a cookie is doing
* it tells the server, "you already asked me for my username and password. now just let me in."

But unlike stamp, which can be easily copied or transferred or duplicated
these cookies are really big and seemingly random values, letters and numbers
* statistically, it's no way someone can guess our cookie value and pretend to be us
* at least it's very low probability

In short, cookie works because the browser and the server have this agreement to send these values back and forth in this way
___

# Implement session - Log in

now it's time to translate the ideas to code
this time let's make a simple login feature

again let's get to `app.py`
```python
from flask import Flask, redirect, render_template, request, session
from flask_session import Session

# Configure app
app = Flask(__name__)

# Configure session
app.config["SESSION_PERMANENT"] = False
app.config["SESSION_TYPE"] = "filesystem"
Session(app)
...
```

`app.config["SESSION_TYPE]" = "filesystem"`
* actually this is not the only way to implement sessions
	* server can store cookies in a database, in a file, in memory, other places too
* we tell Python to store the cookies on the server's hard drive
	* so a folder called Flask_session would suddenly appear when we start storing cookies

`app.config["SESSION_PERMANENT]" = False`
* this time we don't want it to be permanent
* when we close the browser, the session goes away

`Session(app)`
* tells Python to support `Session` for the `app`

That's it for now. Let's try the web application before diving deeper.
1. the login page just ask for the user's name, and have a button to submit the form to Log In
2. if we go to the default route, the slash `/` route which is where most websites live by default, like https://www.example.com/, redirect the user to the `/login` route
	* somehow the code knows, if a user is not logged in, sends him to the log in page
3. when we logged in, we're back to the slash `/` route
4. if we closed the tab, and go to the website again with a new tab, this time we're automatically logged in
5. the page have a hyperlink for users to log out, send the user back to the log in page when the user clicked on that link
	* surely we also make the server forgets about the user, by removing the cookie

firstly, to redirect user to the log in page, our `"/"` route in `app.py` is like this
```python
...
@app.route("/")
def index():
	if not session.get("name"):
		return redirect("/login")
	return render_template("index.html")
...
```

what's in `"index.html"`?
```html
{% extends "layout.html" %}

{% block body %}
	
	{% if session["name"] %}
		You are logged in as {{ session["name"] }}. <a href="/logout">Log out</a>.
	{% else %}
		You are not logged in. <a href="/login">Log in</a>.
	{% endif %}
	
{% endblock %}
```

* again, Jinja does not rely on indentation
	* recall HTML and CSS don't really care about indentation
	* only the human does
* but, in replace, we need these end tags
	* `{% endblock %}`, `{% endfor %}`, `{% endif %}`

* `session` is a magic variable that we now have access to
* because we have `Session(app)` in our `app.py`
	* that handle the whole process of setting every user's cookie, with a unique identifier
	* every user that logged in, has a different `session`
* it's stored in the `flask_session/` directory in the server computer
	* one `session` file for each logged in user
		* inside the file is the `name` the user inputted
	* it creates the illusion of *per user storage*

here is how it looks like in code
```python
...
@app.route("/login", methods=["GET", "POST"])
def login():
	if request.method == "POST":
		session["name"] = request.form.get("name")
		return redirect("/")
	return render_template("login.html")
```

our inference is that if a user got to this route via `POST`,
they must have submitted a form
* that's how we plan to design the HTML `<form>`

`session["name"] = request.form.get("name")`
* if they did got to the route via `POST`, store in the session, at the key `"name"`, the inputted `"name"`

so how is the `<form>` designed?
```html
...
	<body>
		<form action="/login" method="post">
			<input name="name" placeholder="Name" type="text">
			<input type="submit" value="Log In">
		</form>
	</body>
...
```

notice the `<form>` submits to itself, by `action="/login"`
* from route `"/login"` submits to its same route `"/login"`
* but it's using `method="post"`
	* remember if we do visit a page via URL like https://www.example.com/login
	* it's a `GET`
* so we have one route but for two different types of operations or views
	* as a result we don't need to have both an index route and a greet route
	* instead we have one route `"/login"` that handles both `GET` and `POST`

how about log out?
```python
...
@app.route("/logout")
def logout():
	session["name"] = None
	return redirect("/")
```

looking back at `index.html`
so when the user logged out, it displays `You are not logged in.`

> [!summary]
> Flask can make cookies for users and store that in both the server computer and client computer. Whenever Flask sees the same cookie coming back from a user, it grabs the appropriate file from `flask_session/` and loads it into the `session` global variable. So that the code is now unique to that user and their `name`.

____

# eg. Shopping Cart

here comes another example, let's make a online book store
but it does only add items from cart

1. let's see how the page works first, it's `book.html`
```html
{% extends "layout.html" %}

{% block body %}

	<h1>Books</h1>
	{% for book in books %}
		<h2>{{ book["title"] }}</h2>
		<form action="/cart" method="post">
			<input name="id" type="hidden" value="{{ book['id'] }}">
			<input type="submit" value="Add to Cart">
		</form>
	{% endfor %}

{% endblock %}
```

`<input name="id" type="hidden" value={{ book['id'] }}>`
* it will put the value in the form but not reveal it to the user

2. where are `books` come from? sure enough it's `app.py`. let's see how it looks.
```python
from cs50 import SQL
from flask import Flask, redirect, render_template, request, session
from flask_session import Session

# Configure app
app = Flask(__name__)

# Connect to database
db = SQL("sqlite:///store.db")

# Configure session
app.config["SESSION_PERMANENT"] = False
app.config["SESSION_TYPE"] = "filesystem"
Session(app)

@app.route("/")
def index():
	# so here is where `books` in `book.html` comes from
	books = db.execute("SELECT * FROM books")
	return render_template("books.html", books=books)
...
```

3. how the database looks?
```sqlite
sqlite> .schema
CREATE TABLE books (id INTEGER, title TEXT NOT NULL, PRIMARY KEY(id));
```

4. the `<form>` is submitted to route `"/cart"`, so how does the route works?
```python
...
@app.route("/cart", methods=["GET", "POST"])
def cart():
	
	# Ensure cart exists
	if "cart" not in session:
		session["cart"] = []
	
	# POST
	if request.method == "POST":
		id = request.form.get("id")
		if id:
			session["cart"].append(id)
		return redirect("/cart")
	
	# GET
	books = db.execute("SELECT * FROM books WHERE id IN (?)", session["cart"])
	return render_template("cart.html", books=books)
```

`if "cart" not in session:`
* if the key `"cart"` is not in `session`
* then `session["cart"] = []` just assign an empty list as a value to that key
* it makes sure the global variable `session` has a key `"cart"`, by default its value is an empty list
	* meaning the user has an empty shopping cart

`if id:`
* makes sure there is a valid `id`
* users can't muck with the form to append an item with no `id` to the cart
* also `session["cart"]` is a list of book's `id` if you havn't notice already

how about when user access to `"/cart"` via URL directly, that is the `GET` method?
* just show the user what's in his or her shopping cart


extra.

why support both `GET` and `POST`?
* the URL is cleaner, and it's actually a thing.
* like if both adding items to cart and previewing the cart, lead us to the same page `cart.html`, why we should have two seperate route, `"/add_item"` and `"/view_cart"`, instead of just `"/cart"`
___

# eg. search

last example, let's bring back the search thing

1. we have a database, containing some TV shows
	* only 2 columns, `id` and `name` of the shows
2. the search page is just a input field with a submit button
3. when the button is clicked, lead users to the result page which basically is just a unordered list `<ul>` containing the search result, the names of some TV shows
	* the route is `"/search"` and we used URL parameter and `GET` method, much like Google
	* https://www.google.com/search?q=cats

nothing special so far, basic stuffs
next up, how about implementing an autocomplete feature?
* we want the search to be immediate
* we don't even want the submit button
* we don't want to load a second page for search result, just do searching in the same page

let's see the `index.html` first
```html
...
	<body>
		
		<input autocomplete="off" autofocus placeholder="Query" type="search">
		
		<ul></ul>
		
		<script>
			
			let input = document.querySelector('input');
			input.addEventListener('input', async function() {
				let response = await fetch('/search?q=' + input.value);
				let shows = await response.text();
				document.querySelector('ul').innerHTML = shows;
			});
			
		</script>
		
	</body>
...
```

1. when we provide any kind of input, by typing, by pasting, by any other mechanism, it triggers an event called `'input'`
	* kinda like key press or key up

2. skip the `async` and `await` for now and just focus on the function and the idea
* `fetch()` allows us to `GET` or `POST` information to a server without reloading the page
	* it's like secretly reloading the page inside of the page
* it's result is stored as `text()` in `shows` and the next line displayed in the empty `<ul>` element

3. again, we can open up Developer Tools and see from Network tab, the HTTP request
* type anything in the input field and it'll immediately trigger a HTTP request
	* eg. https://www.example.com/search?q=c
* the Response tab contains the raw response gave to us for this request
	* and it is just a bunch of list item `<li>` and inside of which are the TV show names
	* nothing else
* actually if we do visit this URL the browser would try to present them as a complete web page but in fact if we view the source code it's just `<li>` tags only

4. notice, the URL never change
___

# JSON
*JavaScript Object Notation*

next up, can we remove the stupid `<li>` tags?
it feels like we should not create the tags in server and then send them, but create the tags in client

if we visit https://www.example.com/search?q=cat
what comes back should be a JSON file, which look likes this

```json
[
	{
		"id": 63881,
		"title": "Catweazle"
	},
	{
		"id": 65307,
		"title": "Josie and the Pussycats"
	},
	...
]
```

here we formatted it a little nicer, in the real world it'd be much more compact
* maybe just a single line
* it's cryptic for you and me, but very friendly to machine

JSON uses JavaScript syntax
* a square bracket `[]` means here comes an array
* a curly bracket `{}` means here comes an object, aka. a dictionary

It's a very generic format that an API, an *application programming interface*, might return to us
* it's how APIs nowadays work
	* we get back very raw textural data in JSON format
	* and we write code that turns JSON data into any language we want
		* eg. HTML

so let's see how our code does this
```html
...
	<body>
		...
		<script>
			
			let input = document.querySelector('input');
			input.addEventListener('input', async function() {
				let response = await fetch('/search?q=' + input.value);
				let shows = await response.json();
				let html = '';
				for (let id in shows) {
					let title = shows[id].title.replace('<', '&lt;').replace('>', '&gt;');
					html += '<li>' + title + '</li>';
				}
				document.querySelector('ul').innerHTML = html;
			});
			
		</script>
	</body>
...
```

`let shows = response.json();`
* before we used `response.text()` to convert the response to text format
	* this time we parse it to json format
	* turns it into a dictionary, or really a list of dictionaries
* both comes with JavaScript these days
___

> [!summary]
> HTML and CSS are used to present the data, so-called view.
> Python might be used to send or get the data on the backend server.
> JavaScript is going to be used to make things dynamic and interactive.

___
