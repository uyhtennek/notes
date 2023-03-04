# `<form>`

with Flask and URL parameters we have the ability to implement some dynamic things in HTML
but still, URL parameter is a bad user interface
* none of us ever manually change the URL to do Google search
* what is the mechanism for getting input from the user and putting it in URL automatically?

`<form>` tag is how we typically get input from the user
* whether it's a button or a text box or a dropdown menu or something else
* so first thing let's get our `<form>` ready

```html
...
	<body>
		<form action="/greet" method="get">
			<input name="name" placeholder="Name" type="text">
			<input type="submit">
		</form>
	</body>
...
```

`<form action="/greet" method="get">`
* recall [[CS50x 8-5 URL parameter#Implementing a search feature|last time]] we used `action="https://www.google.com/search"` as our so-called backend
	* it sends the user to that route when the form is submitted
* this time we have a backend of ourselves, with the route being `"/greet"`

what would happen now if the user submit the form?
* the user would go to an empty page since we don't have a page for `"/greet"` yet
	* `404 Not Found`
* but what is the URL would look like?
* it would ends with `"/greet?name=Davild"`, where `David` is what the user submitted
___

# `500 Internal Server Error`

so now we have our URL parameter ready,
next thing is get the backend to look at it and do something about it

so let's get back to `app.py`, and add a new function
```python
...
@app.route("/greet")
def greet():
	name = request.args.get("name")
	return render_template("greet.html", name=name)
```

notice before we implement this function we would encounter `404 Not Found`
now if we go to the route `https://www.example.com/greet?name=David`, what would happen?
* it gives us `500 Internal Server Error`
	* the route is actually found, so this time it's not `404 Not Found`
* it means something is wrong with our code

by default there are *logs* in the terminal when the server is running,
showing the requests the server is getting or what is the server executing
* therefore there may be some helpful stuffs left there when there is an error
* this time, at the bottom of terminal, we could find something like

```
...
jinja2.exceptions.TemplateNotFound: greet.html
127.0.0.1 - - [08/Nov/2021 19:05:28] "GET /greet?name=David HTTP/1.0" 500 -
```

so the problem obviously is we don't have `"greet.html"`
after we created it, then it'll be okay
___

# Empty input

another problem is that
what if the user didn't provide a value for `name`?
* the code can still work fine
* just this is not our expected behavior

* we can do something with the `index.html` to require the user to provide input
* we can have some error check
* providing a default value for `name`

let's try the default value method first, so let's get to our `greet()` function again
```python
...
@app.route("/greet")
def greet():
	name = request.args.get("name", "world")
	return render_template("greet.html", name=name)
```

* actually it's quite common to see a `get()` function with Python dictionaries that allows us to supply a default value

The thing is, when the user submit the form with no input, the URL is like:
`https://www.example.com/greet?name=`
* but this actually do have a value, assigned to `name`, which is `""`
* if we go for this approach the URL needs to be `https://www.example.com/greet`
	* without the key value pair `name=`

A probably the most robust way is force the user to provide input
so this time let's get to our `index.html`
```html
...
	<body>
		<form action="/greet" method="get">
			<input name="name" placeholder="Name" required type="text">
			<input type="submit">
		</form>
	</body>
...
```
* added the `required` attribute

Now if we input nothing for `name`,
the browser would yell at us and would not submit the form

However, remember, never trust the users
* error checking like this alone is not safe
> [!quote]
> Never, ever, ever rely on client side safety checks.
* anyone can just open up Developer Tools and delete the `required` attribute
___

# Templating

now we have 2 html files already, we would soon start making bigger websites with tens or hundreds of template
We don't really want to repeat things like `<!DOCTYPE html>` `<html lang="en">`, `<head>`, `<meta>`, etc.
* that is too much repeatition
* suppose we want to change one thing in the `<head>`, we need to do the same in all the others html files

in programming generally we can factor out commonalities
in web programming, and specifically *templating*, we can do the same


1. Firstly, get to the `templates/` file and create a html file
	* for example we named it `layout.html`

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta name="viewport" content="initial-scale=1, width=device-width">
		<title>hello</title>
	</head>
	<body>
		{% blcok body %}{% endblock %}
	</body>
</html>
```

2. so now `index.html` doesn't need all these part anymore, 
    let's just focus on what's different, which is the `<form>`
```html
{% extends "layout.html" %}

{% block body %}

	<form action="/greet" method="get">
		<input name="name" placeholder="Name" required type="text">
		<input type="submit">
	</form>

{% endblock %}
```

this is now a mix of HTML, with *templating code*
* between `{% block body %}` and `{% endblock %}` we tell Flask "Hey, here is my body block"
* by `{% extends "layout.html" %}` we tell Flask to plug the block to the `"layout.html"` placeholder
	* so by contrast `layout.html` is almost all HTML but there is the placeholder `{% blcok body %}{% endblock %}`
	* we can actually put default value there, by typing `{% blcok body %}default{% endblock %}`
		* maybe some pages don't have a body block
		* although in general that's not going to be relevant

3. do the same to `greet.html`
```html
{% extends "layout.html" %}

{% block body %}

	hello, {{ name }}

{% endblock %}
```

These syntax, all these curly bracks `{}` and percent signs `%`, they are not HTML
technically, it's an exmaple of *Jinja syntax*
* which is a language for templating
* later Flask people decided they're not going to come up with some new syntax, just use Jinja
	* in computing there are a lot of sharing, of ideas and of code
	* so other libraries and other people might also too use Jinja syntax

extra.
* pay a little attention to the indentation though, just to see if the code looks pretty in browser
	* but it's not a relevant and important thing
* this is no longer just a web site, it's a web application
* this design is also good for memory
	* theoreticaly because the server knows there is a common layout, it can do some optimizations underneath the hood
	* Flask is probably doing that, but not in the mode we're using it, which is development mode
		* it keeps reloading things each time
___

# POST

But how we implemented this is not the best design for users, why? Privacy.
* we used URL parameter as the user input
* the `name` is visible in the URL and in browser's history
	* other people go trolling with our laptop and trolling through our autocomplete or history, can easily see the inputs
	* also in incognito mode doesn't save that, the URL and URL parameters are still exposed
* what if it's username or password or credit card?

The solution is rather simple
by changing the `<form>` submit `method` from `get` to `post`
```html
{% block body %}

	<form action="/greet" method="post">
		...
	</form>

{% endblock %}
```

now if we do get back to our web page and submit the form
yes our input won't be shown in the URL, it's something like `https://www.example.com/greet`

but we would have an error after we submitted the form
`405 Method Not Allowed`
* that is because Flask only support the `get` method by default
* we need to modify the route for `"/greet"` a bit to support the `post` method
	* modify the [[CS50x 9-1 Flask#Flask setup|decorator]]

```python
@app.route("/greet", methods=["POST"])
def greet():
	name = request.args.get("name", "world")
	return render_template("greet.html", name=name)
```

now if we submit the form, what can we see? what is the value of `name`?
* actually it's `"world"`, no matter what the user input is
	* we need to tweak the backend to look deeper in the `POST` request to get the user input
		* we need to use `request.form` instead of `request.args`
		* other things, still the same
	* since it's not as simple as looking at the URL any more
* but the user input is not shown in the URL any more

Okay so `POST` is good for privacy
then why not always use `POST`? why the `GET` method is there?
* we can, but if we put nothing in the URL, our history or autocomplete gets much less useful
* we can't just get back to where we are previously. we have to re-fill out the form every time.

If we just reload the page, the browser would give us an alert saying *Confirm Form Resubmission*
* that is saying the browser is about to re-submit the same information that we input previously, and then send a new request to the server
	* browser might remember our inputs and that's great, but that's just while we're on the page
* `GET` in contrast, the *states*, our inputs, are embedded in the URL

let say we want to check the time using a URL, maybe we can put it in an email to somehow make it useful for others reading the email
* we can put https://www.google.com/search?q=what+time+is+it
	* since Google supports `GET` request, we can immediately we can check the time
* without Google supporting `GET`, all we can do is only https://www.google.com/search
	* which gives no useful information. bad for usability.
* so we care not only about the design of the low level code, but also the design when it comes to the user experience, or *UX*
___

# MVC

> [!quote]
> Flask is just an example of many different frameworks that all implement the same paradigm, the same way of thinking and the same way of programming applications. And that's known as MVC, model view controller.

![[mvc.png|600]]
actually this is what we're doing when using Flask

* `app.py` is the *controller*
	* all the logic happens there
	* decides what to render, what values to show, and so forth

* `layout.html`, `index.html`, `greet.html`, they are the *view* templates
	* the visualizations that the human actually sees
	* the user interface
	* all they do is simply showing something, ploping some values here and there
		* all of the hard work is done in `app.py`

* we havn't added an *model* yet
	* that refers to things like CSV files or databases
	* where do we keep actual data, typically long term

This is a very common paradigm not only Python and Flask would use
but also Java, C#, or others as well
___
