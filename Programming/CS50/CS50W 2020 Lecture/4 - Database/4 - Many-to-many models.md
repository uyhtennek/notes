# Set-up

we continue to make the app a little more sophisticated
* we want to make a page to display the details of a flight

`urls.py`
```python
urlpatterns = [
	path("", views.index, name="index"),
	path("<int:flight_id>", views.flight, name="flight")
]
```

`views.py`
```python
def flight(request, flight_id):
	flight = Flight.objects.get(pk=flight_id)
	return render(request, "flights/flight.html", {
		"flight": flight
	})
```

`Flight.objects.get(pk=flight_id)`
* `pk` stands for primary key

create the `templates/flights/flight.html`
```html
{% extends "flights/layout.html" %}

{% block body %}
	<h1>Flight {{ flight.id }}</h1>
	<ul>
		<li>Origin: {{ flight.origin }}</li>
		<li>Destination: {{ flight.destination }}</li>
		<li>Duration: {{ flight.duration }}</li>
	</ul>
{% endblock %}
```
___

# Add passengers

`models.py`
```python
class Passenger(models.Model):
	first = models.CharField(max_length=64)
	last = models.CharField(max_length=64)
	flights = models.ManyToManyField(Flight, blank=True, related_name="passengers")
	
	def __str__(self):
		return f"{self.first} {self.last}"
```

`models.ManyToManyField(Flight, blank=True,`
* `blank=True` to allow that a passenger has no flight
* we can use `<passenger>.flights` to see all the flights that this passenger on
* `related_name="passengers"` so that we can use `<flight>.passengers` to see all the passengers on a flight

make and apply the changes
```bash
python manage.py makemigrations
python manage.py migrate
```

register the model in the admin app, in `admin.py`
```python
admin.site.register(Passenger)
```

then we can start adding passengers in the admin app
___

# Show passengers
we want to show the passengers on the flight, in the flight page

`views.py`
```python
def flight(request, flight_id):
	flight = Flight.objects.get(pk=flight_id)
	return render(request, "flights/flight.html", {
		"flight": flight,
		"passengers": flight.passengers.all()
	})
```

`templates/flights/flight.html`
```html
{% extends "flights/layout.html" %}

{% block body %}
	<h1>Flight {{ flight.id }}</h1>
	<ul>
		<li>Origin: {{ flight.origin }}</li>
		<li>Destination: {{ flight.destination }}</li>
		<li>Duration: {{ flight.duration }}</li>
	</ul>
	
	<h2>Passengers</h2>
	<ul>
		{% for passenger in passengers %}
			<li>{{ passenger }}</li>
		{% empty %}
			<li>No passengers.</li>
		{% endfor %}
	</ul>
	
	<!-- in addition we make some links to make navigation between pages easier -->
	<a href="{% url 'index' %}">Back to Flight List</a>
{% endblock %}
```
___

# Booking flights
we want to have a route for booking a flight for a passenger

`urls.py`
```python
urlpatterns = [
	path("", views.index, name="index"),
	path("<int:flight_id>", views.flight, name="flight"),
	path("<int:flight_id>/book", views.book, name="book")
]
```

`views.py`
```python
def book(request, flight_id):
	if request.method == "POST":
		flight = Flight.objects.get(pk=flight_id)
		passenger = Passenger.objects.get(pk=int(request.POST["passenger"]))
		passenger.flights.add(flight)
		return HttpResponseRedirect(reverse("flight", args=(flight.id,)))
```

generally, anytime we want to manipulate the state of something, especially the database, that should be inside of a post request

`int(request.POST["passenger"])`
* post data submitted by default is gonna be string type
* so we need to convert it, if necessary

ultimately we want to do some error checking here
* eg. what if the passenger passed in doesn't exist, or the flight doesn't exist?
* we will skip that for now, for simplicity

`passenger.flights.add(flight)`
* it's the equivalent of adding a new row into the table that keeping track of which passengers are on which flights
* notice that we don't have to worry about the structure of the database and the tables
	* because Django's abstractions take care of that details for us
	* allowing us to think at a much higher level

`args=(flight.id,)`
* it's like this because the `args` parameter takes a tuple

next, we need to make the form, since we didn't make it yet
* in `templates/flights/flight.html`
```html
<h2>Add Passenger</h2>
<form action="{% url 'book' flight.id %}" method="post">
	{% csrf_token %}
	<select name='passenger'>
		{% for passenger in non_passengers %}
			<option value="{{ passenger.id }}">{{ passenger }}</option>
		{% endfor %}
	</select>
	<input type="submit">
</form>
```

`<select name='passenger'>`
* our `book` route is expecting a data with the name `'passenger'`

`views.py`
```python
def flight(request, flight_id):
	flight = Flight.objects.get(pk=flight_id)
	return render(request, "flights/flight.html", {
		"flight": flight,
		"passengers": flight.passengers.all(),
		"non_passengers": Passenger.objects.exclude(flights=flight).all()
	})
```

`Passenger.objects.exclude(flights=flight).all()`
* get all the passengers, excluding the ones who are already on the flight
___
