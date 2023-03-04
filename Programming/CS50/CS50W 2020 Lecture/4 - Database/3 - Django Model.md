where Django gets all the more powerful, in designing web application, is the ability to represent data in terms of Django models
___

# Django set-up

1. create a Django project
```bash
django-admin startproject airline
```

2. create an app
```bash
python manage.py startapp flights
```

3. add the app to `settings.py`
```python
INSTALLED_APPS = [
	'flights',
	...
]
```

4. add the path in `urls.py`
* remember to do `from django.urls import include, path`
```python
urlpatterns = [
	path("flights/", include("flights.urls"))
]
```

5. create `urls.py` for the `flights` app
```python
from django.urls import path
from . import views

urlpatterns = [
	
]
```
___

# Django Models

6. create the models
* model is a Python class, that is going to represent some data
* and we will tell Django to store them inside of a database
	* later Django will figure out what SQL syntax it needs to use
		* to create the table
		* to manipulate the table

In any app created, there is a file, `models.py`
* we define the models to use for the application here

```python
from django.db import models

# Create your models here.
class Flight(models.Model):
	origin = models.CharField(max_length=64)
	destination = models.CharField(max_length=64)
	duration = models.IntegerField()
```
___

# Migrations
basically it just means, telling Django to update the database

it is a two-steps process
* create the migration, which states the changes
* apply the changes to the underlying database

7. we start making migrations by running a command
```bash
python3 manage.py makemigrations
```

then we can see it says
```bash
Migrations for 'flights':
  flights/migrations/0001_initial.py
	  - Create model Flight
```

* if we go to that directory, we can see `0001_initial.py` is indeed created for us
	* it's a file telling Django how it should manipulate the database, to reflect the changes we have made to the model
```python
...
	operations = [
		migrations.CreateModel(
			name='Flight',
			fields=[...]
		)
	]
```

8. apply the migrations, by running
```bash
python3 manage.py migrate
```

there are a bunch of default migrations get applied as well
* but one of them being ours
```bash
...
  Applying flights.0001_initial...OK
  ...
```

if we look into our root directory, we now can see a `db.sqlite3` file
___

# Making changes

surely we can run SQL commands directly to the `db.sqlite3` to update the database
but Django provides some nice abstraction layers
* so we can work with Python classes and variables instead of SQL

9. enter Django's shell to begin with
```bash
python3 manage.py shell
```

it opens up a shell or a console
* we write Python commands here
* and the commands we wrote here will get executed on the web application

* `Ctrl + D` to exit the shell

10. write our instructions
```python
>>> from flights.models import Flight  # import our model
>>> f = Flight(origin="New York", destination="London", duration=415)  # create a flight
>>> f.save()  # save the created flight
```

* when we execute these commands, Django knows to write the right SQL commands to create our created flight model on the underlying SQL table

11. we can query a flight by entering
```python
>>> Flight.objects.all()
```

* it's equivalent to "select all" or `SELECT *`

we will get
```python
<QuerySet [<Fight: Flight object (1)>]>
```

`QuerySet`
* a set of results from a query

`<Flight: Flight object (1)>`
* only 1 flight came back to us
* its ID is 1
___

## object name

it might not be ideal that we can only see flights by their ID
it turns out, we can give names to them

any model can implement a special function `__str__`
* it returns a string representation of a particular object
* actually, it applies to not just Django models, but to Python classes
```python
# file: models.py
from django.db import models

# Create your models here.
class Flight(models.Model):
	origin = models.CharField(max_length=64)
	destination = models.CharField(max_length=64)
	duration = models.IntegerField()
	
	def __str__(self):
		return f"{self.id}: {self.origin} to {self.destination}"
```

now if we enter the Django's shell again
```bash
python3 manage.py shell
```

and select all the flights
```python
>>> from flights.models import Flight
>>> flights = Flight.objects.all()
>>> flights
```

it outputs
```python
<QuerySet [<Flight: 1: New York to London>]>
```


## more Django's commands

additionally, we can get just that my flight instead of all flights
```python
>>> flight = flights.first()
>>> flight
```

it outpus
```python
<Flight: 1: New York to London>
```

we can access the object's properties as well
```python
>>> flight.id
1
>>> flight.origin
'New York'
>>> flight.destination
'London'
>>> flight.duration
415
```

and we can delete it
```python
>>> flight.delete()
(1, {'flights.Flight': 1})
```
___

# Relational Models

but this is not we want to represent the flights remember?
* we would like to use airport IDs, instead of city names

so we need to create the airport model
* again, in `models.py`
```python
class Airport(models.Model):
	code = models.CharField(max_length=3)
	city = models.CharField(max_length=64)
	
	def __str__(self):
		return f"{self.city} ({self.code})"

class Flight(models.Model):
	origin = models.ForeignKey(Airport, on_delete=models.CASCADE, related_name="departures")
	destination = models.ForeignKey(Airport, on_delete=models.CASCADE, related_name="arrivals")
	duration = models.IntegerField()
	
	def __str__(self):
		return f"{self.id}: {self.origin} to {self.destination}"
```

`models.ForeignKey()`
* only the first argument is necessary, for making that foreign key
* `models.ForeignKey(Airport)` is enough

`on_delete=models.CASCADE`
* since the column is related to another table, SQL needs to know what should happen to the data if the reference get deleted
	* eg. we have a flight flying to JFK, but then for some reason we decided to delete the airport JFK, what should happen to the flight data?
* `models.CASCADE` means if the an airport get deleted, any corresponding flight also get deleted
	* `models.PROTECT` on the other hands mean the opposite, don't delete the flight

`related_name="departures"`
* it allows us to access a relationship, in the reverse order
* eg. normally we ask the question, "what's the `origin` of a particular flight?"
	* sometimes, we want to ask the question in reverse order, "what are the flights that have an `origin` of JFK or some other airports?"
		* so "what are the `"departures"` from a given airport?"
* by giving a `related_name`, Django will automatically set up the relationship going in that reverse order

Then, again, we need to make and apply the migrations
```bash
python manage.py makemigrations
python manage.py migrate
```

Then we enter the Django's shell and create some airports
```bash
python manage.py shell
```

```python
jfk = Airport(code="JFK", city="New York")
jfk.save()
lhr = Airport(code="LHR", city="London")
lhr.save()
...
```

then create a flight
```python
f = Flight(origin=jfk, destination=lhr, duration=415)
f.save()
```

and we can access to different properties of a flight or airport
```python
>>> f
<Flight: 1: New York (JFK) to London (LHR)>
>>> f.origin
<Airport: New York (JFK)>
>>> f.origin.city
'New York'
>>> f.origin.code
'JFK'
>>> lhr.arrivals.all()
<QuerySet [<Flight: 1: New York (JFK) to London (LHR)>]>
```
___

# Continue building

`urls.py`
```python
urlpatterns = [
	path("", views.index, name="index")
]
```

`views.py`
```python
from django.shortcuts import render
from .models import Flight

def index(request):
	return render(request, "flights/index.html", {
		"flights": Flight.objects.all()
	})
```
* then we need to create the template

`templates/flights/layout.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Flights</title>
	</head>
	<body>
		{% block body %}
		{% endblock %}
	</body>
</html>
```

`templates/flights/index.html`
```html
{% extends "flights/layout.html" %}

{% block body %}
	<h1>Flights</h1>
	<ul>
		{% for flight in flights %}
			<li>Flight {{ flight.id }}: {{ flight.origin }} to {{ flight.destination }}</li>
		{% endfor %}
	</ul>
{% endblock %}
```

finally we can run the server to see it in action
```bash
python manage.py runserver
```
___

# More Django queries

```python
# get all airports
>>> Airport.objects.all()
<QuerySet [<Airport: New York (JFK)>, <Airport: London (LHR)>, ...]>
# get all airports whose city is New York
>>> Airport.objects.filter(city="New York")
<QuerySet [<Airport: New York (JFK)>]>
# get the first airport whose city is New York
>>> Airport.objects.filter(city="New York").first()
<Airport: New York (JFK)>
# get the only result from a query, if there is only one
# if there is none or more than one result, it throws an error
>>> AIrport.objects.get(city="New York")
<Airport: New York (JFK)>
```

play around
```python
>>> jfk = Airport.objects.get(city="New York")
>>> cdg = Airport.objects.get(city="Paris")
>>> f = Flight(origin=jfk, destination=cdg, duration=435)
>>> f.save()
```
___

# Django Admin

Surely we don't want to get to the shell to do the data manipulation
* it's still manual, it's annoying

Django is built on this idea that it doesn't want us to repeat work
* and this process of define models and create, edit, manipulate models is so common
* so Django has already built for us an entire app, that is just designed to manipulate models
	* it's known as the Django Admin app

that's why the `admin/` path is there in `urls.py`
```python
...
urlpatterns = [
	path('admin/', admin.site.urls),
	... # our own apps
]
```


## How to use?

1. create an administrative account
* of our Django web application
```bash
python3 manage.py createsuperuser
```
* then enter your username, email address, and password

what a super user can do is
* visit the web interface for the admin app
* and manipulate some of the underlying models

2. for that, we need to take our models and add them to the admin app
* by adding them to `admin.py`
```python
from django.contrib import admin
from .models import Flight, Airport

# Register your models here.
admin.site.register(Airport)
admin.site.register(Flight)
```
* it tells Django's admin app that we want to manipulate `Airport` and `Flight` data

3. run and login and use the admin app
* start the server, go to the `admin/` path, log-in
* there we can quickly see and create and edit data with registered models
___

> [!info] Django origin
> Django was originally created for news organization. They need to very quickly to be able to post articles and post new posts on their website. So Django made an interface for these tasks.
