Django is a Python web framework
* which can dynamically generate HTML and CSS
* so that ultimately, it allows us to build a dynamic web application
___

# Web Applications

by just using HTML and CSS to create web pages, the pages are static
* they are the same every time we visit the web page
* every time, the page is identical

But, that would be rarely the case in our daily life websites
* eg. New York Times home page probably is different every day
	* and that's probably automated
	* we wouldn't see a guy updating the HTML and CSS every day

Using these web framework, we can create software that's going to run on a web server, such that clients can make requests to our server, and our server is going to respond back with some dynamically generated content.
___

# HTTP
HyperText Transfer Protocol
it's a protocol of how messages are sent back and forth over the Internet

requests from clients typically look something like this
```
GET / HTTP/1.1
Host: www.example.com
...
```

`Get` is a request method
`/` is the page the client is trying to get, which is the default page or home page of the website
`HTTP/1.1` is the version of HTTP that we're using
`www.example.com` is the host, or the computer, that we're trying to access to

a response typically look something like this
```
HTTP/1.1 200 OK
Content-Type: text/html
...
```

`200` is a response code, which just means OK
the format of the data that's coming back in this response is `text/html`, a HTML data
* the web browser on the client should then render a HTML


## HTTP Status Codes

| Status Code | Description |
| :-: | :-: |
| 200 | OK |
| 301 | Moved Permanently |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |
___

# Django Installation

1. first thing is to install Django
* by running `pip3 install Django`

2. then we can create a Django project
* by running another command, `django-admin startproject PROJECT_NAME`
* then Django will automatically create some starter files for us

`manage.py`
* we generally don't need to touch this file
* but we need this file so that we can execute commands on the Django project

`settings.py`
* contains important configuration settings for the web app
* it's preloaded with some default settings right now

`urls.py`
* it's like a table of contents for our web application
* it contains a number of different URLs or routes on our web application

3. now we can start the server to see how the web app looks like
* by running yet another command, `python manage.py runserver`
* it will give us a debug message, at the end there is this
```bash
Starting development server at http://127.0.0.1:8000/
```

`127.0.0.1` is an IP address, which refers to our computer
* the server is running on this IP, meaning right now only our computer can visit this website, but not anyone else on the Internet

`8000` is a port number, which refers to what type of service is being run
* often times we would have multiple different Internet services running on different ports

so when we visit `http://127.0.0.1:8000/` with a web browser, it gives us the Django default page
* saying that Django is loaded already, so we can start building the web application
___

# Django Application
Every Django project generally consists of one or more Django applications

If this is a new concept to you, think about some big websites
* often times a big website is a big project, which has multiple different apps that are sort of separate services that operate within it
* eg. Google has Google Search, Google Images, Google News, Google Maps, .....
	* you can think of each of those individual services, as a separate app
	* but all of them are under one large project, Google
* eg. Amazon has an app for shopping, another for Amazon's video services, ....

Therefore, Django comes preloaded with this ability, to take a project and divide it into multiple distinct apps

4. So we now have a Django project, the first thing we need to do is creating a Django app
* by running `python manage.py startapp APP_NAME`
* then it will create a directory, that's essentially containing the newly created app
	* again, it has several files ready for us

one of them is `views.py`
* we would describe what users will see when they visit a particular route on this app

5. Next we need to install this newly created app to our Django project
* we need to configure the project's `settings.py` now
* find `INSTALLED_APPS` and add the Django app's name there

6. Now we can start building the app, we would start by making it display something
* go to the app's `views.py`
```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def index(request):
	return HttpResponse("Hello, world!")
```

`request` represents the HTTP request that the user made
`HttpResponse` is a class created by Django

so this function responses something, but Django actually don't know when it should run it
* right now, this function just exit, but it doesn't do anything
* we need to tell Django when to run this function, to return this response to users
	* typically, it's more like what URL is the user visiting, to run this function

7. we need to create a `urls.py` file for the app
* so Django has one `urls.py` file for the entire project, but we usually need a `urls.py` file for each app it's containing as well
```python
from django.urls import path
from . import views

urlpatterns = [
	path("", views.index, name="index")
]
```

`urlpatterns` is a list containing all of the allowable URLs of the app and the corresponding views
`views.index` means the `index` function from `views.py`
* `from . import views`, meaning from this directory, import `views.py`
we can give a `name` to a URL, so it's easy to reference it from other parts of the application

8. note that although now we have the route ready in the app, it's not ready in the project
* so, in `urls.py` of the project, we need to add the app
```python
... # long comments
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
	path('admin/', admin.site.urls)  # admin app is a default application from Django
	path('hello/', include("hello.urls"))  # this is the app that's we created
]
```

`include("hello.urls)"` gives the `urls.py` from the `hello` module
* meaning, essentially all the allowable URLs of the hello app

9. now we can start the server and go to https://127.0.0.1:8000/hello/ to see it in action

ex. to add another view and URL to the app
1. add another function in `views.py`
```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def index(request):
	return HttpResponse("Hello, world!")

def brian(request):
	return HttpResponse("Hello, Brian!")
```
2. add the URL to `urls.py`
```python
from django.urls import path
from . import views

urlpatterns = [
	path("", views.index, name="index"),
	path("brian", views.brian, name="brian")
]
```
3. now we can visit http://127.0.0.1:8000/hello/brian/
___

## using URL parameter

eg. on Twitter, we can go to https://twitter.com/USERNAME/ to see someone's tweets
eg. on GitHub, https://github.com/USERNAME/ to see someone's repositories

it seems it would be tedious, because we need to have a function for each user
but actually, it wouldn't be that painful because we can make use of URL parameters

in `views.py`
```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def index(request):
	return HttpResponse("Hello, world!")

def brian(request):
	return HttpResponse("Hello, Brian!")

def greet(request, name):
	return HttpResponse(f"Hello, {name.capitalize()}!"  # capitalize the first letter
```

in `urls.py`
```python
from django.urls import path
from . import views

urlpatterns = [
	path("", views.index, name="index"),
	path("brian", views.brian, name="brian"),
	path("<str:name>", views.greet, name="greet")
]
```

if we visit http://127.0.0.1:8000/hello/harry/
* it will call the `greet()` function
* and generate a response correspond to that URL parameter
___

# Template

`HttpResponse()` can only take a string as the response,
but usually we would like to use a HTML file as a response
* for that we can use the `render()` function

```python
def index(request):
	return render(request, "hello/index.html")
```

we actually need to put the HTML file in a `templates/` folder in the app's directory
* the full path in this case is, `hello/templates/hello/index.html`
* we put it in a `hello/` folder to avoid conflicts if we have multiple `index.html` from different apps?

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Hello!</title>
	</head>
	<body>
		<h1>Hello, world!</h1>
	</body>
</html>
```

but HTML by itself, is static
we actually didn't do any templating if we just render a HTML file, so let's use a template

1. we need to pass some parameters to the `render()` function
```python
def greet(request, name):
	return render(request, "hello/greet.html", {
		"name": name.capitalize()
	})
```

the third argument here, is what we called, *context*
* which is a Python dictionary
* it contains all the information that we would like to provide to the template
	* the template can access `"name"`, and its value is `name.capitalize()`

2. create the `templates/hello/greet.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Hello!</title>
	</head>
	<body>
		<h1>Hello, {{ name }}!</h1>
	</body>
</html>
```

`{{ }}` is part of the Django's templating language
* it allows us to plug in the value of a variable, to this particular position

> [!info] Summary
> When we visit https://127.0.0.1:8000/hello/david/,
> 1. In `hello/urls.py`, we decided the server should run the `greet()` function, taking the HTTP parameter, `david`, as the `name` parameter of the function, as `str` type
> 2. In the `greet()` function, we pass in the `name` argument to the `render()` function, effectively allowing us to pass the information to the HTML
> 3. In `templates/hello/greet.html`, it plugs in the value of the variable accordingly
> 
> It's quite a lot of things happening. But it helps separating parts of the web application, so the structure is actually clearer.

___
