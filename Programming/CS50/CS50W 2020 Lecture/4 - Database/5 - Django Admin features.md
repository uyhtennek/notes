# Admin Configuration

The Django's admin interface actually is quite customizable
* we can do configuration on it

`admin.py`
```python
from django.contrib import admin
from .models import Flight, Airport

# Register your models here.
class FlightAdmin(admin.ModelAdmin):
	list_display = ("id", "origin", "destination", "duration")

class PassengerAdmin(admin.ModelAdmin):
	filter_horizontal = ("flights",)

admin.site.register(Airport)
admin.site.register(Flight, FlightAdmin)
admin.site.register(Passenger, PassengerAdmin)
```

in class `FlightAdmin`, we can specify some settings about how the flight admin page is displayed
* `list_display`: when all the flights are listed, what fields should we have access to?

`admin.site.register(Flight, FlightAdmin)`
* register the `Flight` model, but use the `FlightAdmin` settings

`filter_horizontal`
* make it a little bit nicer to manipulate many-to-many relationship
* it makes a horizontal filter between *Available flights* and *Chosen flights*
___

# Authentication

Django has a whole bunch of authentication features built in
* we don't need to rewrite stuffs like
	* how do you log someone in
	* what does it mean to represent a user

1. we will create another app for this
* it maintains users inside of this application
```bash
python manage.py startapp users
```

`settings.py`
```python
INSTALLED_APPS = [
	'flights',
	'users',
	...
]
```

`urls.py`
```python
urlpatterns = [
	path('admin/', admin.site.urls),
	path("flights/", include("flights.urls")),
	path("users/", include("users.urls"))
]
```

`urls.py` of the `users` app
```python
urlpatterns = [
	path("", views.index, name="index"),
	path("login", views.login_view, name="login"),
	path("logout", views.logout_view, name="logout")
]
```

`views.py`
```python
from django.http import HttpResponseRedirect
from django.shortcuts import render
from django.urls import reverse

# Create your views here.
def index(request):
	# check if user authenticated
	if not request.user.is_authenticated:
		return HttpResponseRedirect(reverse("login"))
	
	# display information about the signed in user
	return render(request, "users/user.html")

def login_view(request):
	return render(request, "users/login.html")

def logout_view(request):
	pass
```

`request.user.is_authenticated`
* the request object in Django automatically has a `user` attribute
* `user.is_authenticated` tells us if the user has signed in or not

`templates/users/layout.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Users</title>
	</head>
	<body>
		{% block body %}
		{% endblock %}
	</body>
</html>
```

`templates/users/login.html`
```html
{% extends "users/layout.html" %}

{% block body %}
	<form action="{% url 'login' %}" method="post">
		{% csrf_token %}
		<input type="text" name="username" placeholder="Username">
		<input type="password" name="password" placeholder="Password">
		<input type="submit" value="Login">
	</form>
{% endblock %}
```

remember usually we would like forms to be submitted via POST method
* especially for the case when username and password are involved
* because GET method will show the form in URL

`templates/users/user.html`
```html
{% extends "users/layout.html" %}

{% block body %}
	<h1>Welcome, {{ request.user.first_name }}</h1>
	<ul>
		<li>Username: {{ request.user.username }}</li>
		<li>Email: {{ request.user.email }}</li>
	</ul>
	<a href="{% url 'logout' %}">Log Out</a>
{% endblock %}
```

`{{ request.user.first_name }}`
* it turns out, in Django, we have access to the request the client made
___

## Creating Users

we can create users in the Django's admin app
* we can get additional information associated to users, besides username and password
* eg. first name, last name, email address, etc.

but actually we can't log in to our `users` app yet
because we didn't have the logic ready
___

## Log In

`views.py`
```python
from django.contrib.auth import authenticate, login, logout

def login_view(request):
	if request.method == "POST":
		username = request.POST["username"]
		password = request.POST["password"]
		user = authenticate(request, username=username, password=password)
		if user is not None:
			login(request, user)
			return HttpResponseRedirect(reverse("index"))
		else:
			return render(request, "users/login.html", {
				"message": "Invalid credentials."
			})
	
	return render(request, "users/login.html")
```

`authenticate`
* checks if the username and password are correct
* if both are correct, it gives back to us, who the user actually is
	* otherwise, `None`

`templates/users/login.html`
```html
{% block body %}
	
	{% if message %}
		<div>{{ message }}</div>
	{% endif %}
	
	... <!-- the login form -->
{% endblock %}
```
___

## Log Out

`views.py`
```python
def logout_view(request):
	logout(request)
	return render(request, "users/login.html", {
		"message": "Logged out."
	})
```

`logout(request)`
* basically it makes `user.is_authenticated` to `False`
___
