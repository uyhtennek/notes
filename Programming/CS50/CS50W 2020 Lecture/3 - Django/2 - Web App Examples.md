# Is It New Year?

eg. https://isitchristmas.com
this is perhaps the simplest website
* say yes when it is christmas
* say no when it is not

so let's make a https://isitnewyear.com
___

## Main Logic

1. we will put it in the same Django project, so just run `python manage.py startapp newyear`

2. in the project's `settings.py`, add the app, `newyear`
3. in `urls.py`, add the path, `path('newyear/', include("newyear.urls"))`

4. we need to create that file, `newyear/urls.py`
```python
from django.urls import path
from . import views

urlpatterns = [
	path("", views.index, name="index")
]
```
5. and we need to create the view, in `views.py`
```python
import datetime
from django.shortcuts import render

def index(request):
	now = datetime.datetime.now()
	return render(request, "newyear/index.html", {
		"newyear": now.month == 1 and now.day == 1
	})
```
6. create `templates/newyear/index.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Is it New Year's?</title>
	</head>
	<body>
		{% if newyear %}
			<h1>YES</h1>
		{% else %}
			<h1>NO</h1>
		{% endif %}
	</body>
</html>
```

`{{ }}` for plugging in variables
`{% %}` for performing logics

7. `python manage.py runserver` to start the server
___

## Static files

However, the web page doesn't have any style right now
* we still would do it by linking CSS file
* but Django has a special system for dealing with static files like this
	* static files are the files that aren't going to change

The HTML file changes accordingly based on whether it's new year or not
The CSS file doesn't change

and since it doesn't change, Django can be a little bit cleverer about it
* if a project is large scale, generally we would like to store the static files somewhere separate, to make them easy to access
	* somewhere they're cached for faster reads later

for that, we need to create a `static/` folder,
and put our `newyear/styles.css` in there
```css
h1 {
	font-family: sans-serif;
	font-size: 90px;
	text-align: center;
}
```

```html
{% load static %}

<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Is it New Year's?</title>
		<link rel="stylesheet" href="{% static 'newyear/styles.css' %}">
	</head>
	<body>
		{% if newyear %}
			<h1>YES</h1>
		{% else %}
			<h1>NO</h1>
		{% endif %}
	</body>
</html>
```

in `<link>`, although we can still directly link to the CSS file,
but Django doesn't like that and has its own best practice

it makes sense to because in larger web applications, where the static files are might change
* maybe we moved the files to a different domain or a different route
* so it's better to let Django figures out where the static files are located and plug in the corresponding URL
	* by typing `{% static <file> %}`

> [!warning] Reminder
> Django need to reload the server to load in the changes of static files

___

# To-Do List
so this is our third app
so do the set-up first

then the main logic, in `views.py`
```python
from django.shortcuts import render

tasks = ["foo", "bar", "baz"]

# Create your views here
def index(request):
	return render(request, "tasks/index.html", {
		"tasks": tasks
	})
```

then the `tasks/index.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Tasks</title>
	</head>
	<body>
		<ul>
			{% for task in tasks %}
				<li>{{ task }}</li>
			{% endfor %}
		</ul>
	</body>
</html>
```
___

## Adding Task

`views.py`
```python
def index(request):
	...

def add(request):
	return render(request, "tasks/add.html")
```

remember to add the path in `urls.py`
* `path("add", views.add, name="add")`

then the `tasks/add.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Tasks</title>
	</head>
	<body>
		<h1>Add Task</h1>
		<form>
			<input type="text" name="task">
			<input type="submit">
		</form>
	</body>
</html>
```
___

## Template Inheritance
notice that we now have 2 HTML files, that're, for a large part, the same

If it was plain HTML, we can't really avoid this
But since we're using Django, there is a solution

firstly we need a `layout.html`, and later both `index.html` and `add.html` will inherit from it
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Tasks</title>
	</head>
	<body>
		{% block body %}
		{% endblock %}
	</body>
</html>
```

`index.html`
```html
{% extends "tasks/layout.html" %}

{% block body %}
	<h1>Tasks</h1>
	<ul>
		{% for task in tasks %}
			<li>{{ task }}</li>
		{% endfor %}
	</ul>
{% endblock %}
```

`add.html`
```html
{% extends "tasks/layout.html" %}

{% block body %}
	<h1>Add Task</h1>
	<form>
		<input type="text" name="task">
		<input type="submit">
	</form>
{% endblock %}
```
___

## Link

now usually we would like to have link in both pages so that it's easy to jump back and forth
* because we don't want to keep typing the URLs

so we would put an anchor type somewhere, writing something like
`<a href="tasks/add.html">Add a New Task</a>`

but, if we have many pages, each of them is linked to `tasks/add.html`
then if we moved `tasks/add.html` to something like `new/add.html`, we would need to change all of those anchor tags

recall we gave ***names to different routes***, in `urls.py`, this is where it becomes relevant
so we would write `<a href="{% url 'add' %}">Add a New Task</a>` instead


### namespace collision

similarly, let's make an anchor tag for the app's homepage
`<a href="{% url 'index' %}">View Tasks</a>`

However, it will take us to http://127.0.0.1:8000/newyear/
* somehow we're on the Tasks app, all the sudden it takes us to the New Year app
* it turns out this is an example of a namespace collision
	* we have two things, that happen to have the same name

> [!question]
> You may wonder why Django allows jumping between different apps.
> But this is quite common actually.
> eg. On the Amazon's shopping page, there might be a link that takes us to Amazon Video.
> eg. Maybe we want to jump from Google Search, to Google Maps.

A easy fix would be to identify each app
* in `urls.py`, give the app a name
```python
from django.urls import path
from . import views

app_name = "tasks"  # this is the new line
urlpatterns = [
	path("", views.index, name="index"),
	path("add", views.add, name="add")
]
```
* in `add.html`, we need to change the link as well
```html
{% extends "tasks/layout.html" %}

{% block body %}
	<h1>Add Task</h1>
	<form>
		<input type="text" name="task">
		<input type="submit">
	</form>
	<a href="{% url 'tasks:index' %}">View Tasks</a>
{% endblock %}
```

`{% url tasks:index %}` means get the `index` URL from the `tasks` app
* likewise, in `index.html`, `<a href="{% url 'tasks:add' %}">Add a New Task</a>`
___

## POST request

right now submitting the form for adding tasks doesn't do any thing
* so we need add logic for handling that

```html
{% extends "tasks/layout.html" %}

{% block body %}
	<h1>Add Task</h1>
	<form action="{% url 'tasks:add' %}" method="post">
		<input type="text" name="task">
		<input type="submit">
	</form>
{% endblock %}
```

recall, anytime we type in a URL or click on a link to go to another page, it is implicitly using the GET request method, meaning, getting a particular page
anytime we're submitting data to change some state inside the application, we generally would use the POST method
* it doesn't include parameters inside the URL

now if we try submitting the form, it gives us this
![[CSRF-verification-failed.png]]
* `403 Forbidden`, meaning, for some reason, we don't have permission to submit the form
* CSRF stands for *Cross-Site Request Forgery*
	* it means there's a security vulnerability in the form
	* someone could forge a request to our website, using some form on their own separate website, for example

eg. let's say we're making a bank web app instead of a to-do list
* someone can trick the user into submitting a form from their website, to our bank app
	* in our app the form means "I would like to transfer money from this user to another user"
	* but in their website the form can mean something else

the solution to this, is that we shouldn't allow for requests to be forged by another website
* by adding a hidden CSRF token into our form
	* which is some unique token that's generated for every session
	* so every time a different user visits this particular form, they see a different CSRF token
* then when the user submit the form, they're submitting the token along with the form
* then in our web application, we're going to check to make sure the token is indeed valid
	* if it's valid, we allow the form submission to go through
* and since adversaries don't know what the generated token is, they can't fake the post request

Django actually has CSRF validation turned on by default, that's why we're forbidden
* it's using an add-on known as Django Middleware
	* Middleware refers to, Django's ability to intervene in the request-response processing
* we can look at the project's `settings.py` for this
```python
...
MIDDLEWARE = [
	...
	'django.middleware.csrf.CsrfViewMiddleware',
	...
]
...
```
* there's a whole bunch of middleware installed by default
	* `CsrfViewMiddleware` is one of them
	* whenever we're submitting data via post, it will do a CSRF validation

but we still need to add CSRF token to our form
* it's easy, we just need to add `{% csrf_token %}` in the form
```html
{% extends "tasks/layout.html" %}

{% block body %}
	<h1>Add Task</h1>
	<form action="{% url 'tasks:add' %}" method="post">
		{% csrf_token %}
		<input type="text" name="task">
		<input type="submit">
	</form>
{% endblock %}
```
* if you're curious and open the Page Source, we can see the line actually is something like
```html
<input type="hidden" name="csrfmiddlewaretoken" value="CUb6ixiJd0a0lfeo4tvI5T...">
```


### Django form
an alternative solution is using forms that are created by Django

in `views.py`
```python
from django import forms
from django.shortcuts import render

tasks = ["foo", "bar", "baz"]

class NewTaskForm(forms.Form):
	task = forms.CharField(label="New Task")

# Create your views here.
def index(request):
	...

def add(request):
	return render(request, "tasks/add.html", {
		"form": NewTaskForm()
	})
```

then in `add.html`
```html
{% extends "tasks/layout.html" %}

{% block body %}
	<h1>Add Task</h1>
	<form action="{% url 'tasks:add' %}" method="post">
		{% csrf_token %}
		{{ form }}
		<input type="submit">
	</form>
{% endblock %}
```

and if we ever would want to change the form, we can do it in the Python code, without touching the HTML file
```python
class NewTaskForm(forms.Form):
	task = forms.CharField(label="New Task")
	priority = forms.IntegerField(label="Priority", min_value=1, max_value=10)
```

another benefit is that Django will do client-side validation automatically
* we don't need to add the `required` attribute manually
___

## Server-side Validation

however, client-side validation is not gonna cut it, we also need server-side validation, because
* it's really easy to disable this sort of client-side validation
* or maybe the user is looking at an old version of the page, and we the only up-to-date validation is on the server

```python
def add(request):
	if request.method == "POST":
		form = NewTaskForm(request.POST)
		if form.is_valid():
			task = form.cleaned_data["task"]
			tasks.append(task)
		else:
			return render(request, "tasks/add.html", {
				"form": form
			})
	
	return render(request, "tasks/add.html", {
		"form": NewTaskForm()
	})
```

`NewTaskForm(request.POST)`
* `request.POST` contains all of the data that the user submitted via the form
* and we populate a `NewTaskForm` object with these data

`form.cleaned_data` gives us access to all of the data that the user submitted

if the form submitted is not valid, we throw back the data that we received to the user
* along with the errors that come up to us as well
* the error is added to the corresponding part of the `form` automatically if somethind is invalid
	* eg. if `priority` max is `5` and somehow user sends us `8`, so the form is invalid, then we send them back the form data inputted along with the error message
	* "Ensure this value is less than or equal to 5."

> [!tip] Routes that support both GET and POST
> Often times pages that have forms will first allow users to get the form, via the GET method, just get the page in order to display the contenst.
> Then it also support a POST method, so that users can post data to those routes, to submit data to that route.

___

## Redirect

right now user will be taken to the `tasks/add.html` after the new task has been added
we might want to redirect users to the homepage

```python
def add(request):
	if request.method == "POST":
		form = NewTaskForm(request.POST)
		if form.is_valid():
			task = form.cleaned_data["task"]
			tasks.append(task)
			return HttpResponseRedirect(reverse("tasks:index"))
		else:
			return render(request, "tasks/add.html", {
				"form": form
			})
	
	return render(request, "tasks/add.html", {
		"form": NewTaskForm()
	})
```

`return HttpResponseRedirect(reverse("tasks:index"))`
* here we can instead use the URL, like `HttpResponseRedirect("/tasks")`
* but a better design would be use a name of the route instead of the URL directly
	* and let Django figure out what the URL should be
* also, actually we need to import both `HttpResponseRedirect` and `reverse`
```python
from django.http import HttpResponseRedirect
from django.urls import reverse
```
___

## Session

now the problem left is that, every user has the same to-do list
so we need sessions

what is sessions?
* one, Django needs it to remember who you are
* but more importantly, Django needs it to store data about your particular session
	* eg. your user ID, your to-do list, ....

```python
...
def index(request):
	if "tasks" not in request.session:
		request.session["tasks"] = []
	return render(request, "tasks/index.html", {
		"tasks": request.session["tasks"]
	})
```

`request.session` is like a big dictionary of data, inside of the session of a user

if we visit the page now, it gives us an error
```
OperationalError at /tasks/
no such table: django_session
```

It turns out, Django tends to store data inside of tables
* so the data is actually not in `request.session`
* and the table is not created yet, so we need to create it
	* by running `python manage.py migrate`
	* it effectively just create all of the default tables, inside of Django's database
		* we can have our own table, but that's for the next time

btw, since the `tasks` initially is empty, it's a little confusing for the user to look at a blank page
we can fix that in `tasks/index.html`
```html
{% extends "tasks/layout.html" %}

{% block body %}
	<h1>Tasks</h1>
	<ul>
		{% for task in tasks %}
			<li>{{ task }}</li>
		{% empty %}
			<li>No tasks.</li>
		{% endfor %}
	</ul>
{% endblock %}
```

next, we need to fix the `add` route as well
```python
def add(request):
	if request.method == "POST":
		form = NewTaskForm(request.POST)
		if form.is_valid():
			task = form.cleaned_data["task"]
			request.session["tasks"] += [task]  # this is the only change
			return HttpResponseRedirect(reverse("tasks:index"))
		else:
			return render(request, "tasks/add.html", {
				"form": form
			})
	
	return render(request, "tasks/add.html", {
		"form": NewTaskForm()
	})
```
___
