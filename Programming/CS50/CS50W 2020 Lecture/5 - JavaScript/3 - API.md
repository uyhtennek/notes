# Local Storage

sometimes we want to store information on the client's local storage
* otherwise, everytime the client close and re-open the web page, everything is set to its initial
* so maybe we would want some way of remembering information so they don't get reset

in later versions of JavaScript and more modern browsers, we can do things just like that
* it's called *local storage*
* it allows us to store information inside of the user's web browser
* later on if the user re-visit the page, it can go back to the storage and extract those values

we have two main functions for doing that
* `localStorage.getItem(key)`
* `localStorage.setItem(key, value)`

`count.js`
```js
// if there is no counter in localStorage, make one
if (!localStorage.getItem('counter')) {
	localStorage.setItem('counter', 0);
}

function count() {
	let counter = localStorage.getItem('counter');
	counter++;
	document.querySelector('h1').innerHTML = counter;
	localStorage.setItem('counter', counter);
}

document.addEventListener('DOMContentLoaded', function() {
	// on page loaded, set the text to what ever counter is
	document.querySelector('h1').innerHTML = localStorage.getItem('counter');
	
	document.querySelector('button').onclick = count;
});
```


## Aplication tab

we can open up Developer Tool, then the Application tab
then we can see there is a section called Storage, and there is a Local Storage > file://

inside of it we can see our stored information
* a key named `counter`, and its value
* we can manipulate it, and delete it, if we want
___

# JavaScript Object

it basically is the equivalent of a Python dictionary
* some association of keys to values

```js
let person = {
	first: 'Harry',
	last: 'Potter'
};

console.log(person.first);
console.log(person["first"]);
// both output 'Harry'
```

one of the way it's most useful, is in the exchange of data
we can move data around, from one service to another
___

# APIs
*application programming interfaces*

in the context of the web, we can think of them as some well-defined structured way, for services on the Internet to communicate with each other
* eg. maybe we want our application to be able to talk to some other service on the Internet, say Google Maps, Amazon, etc., to get some weather information
* we might be able to access some APIs so that we can communicate with the service
	* by sending a request, and receiving back data
	* typically in some sort of very well structured format
	* typically that format would be a type of data, known as JSON
___

# JSON
*JavaScript Object Notation*
* it is a way of transferring data, in the form of JavaScript objects

```json
{
	"origin": "New York",
	"destination": "London",
	"duration": 415
}
```

so, if we wanted our airline service to be able to make its data available to other services or progrms, so they can programmatically access information about flights from us
* we can pass data in this format to those other applications
* then they can treat this JSON data as a JavaScript object, and then get access to the information

the nice thing about this representation is that,
it is both human readable, and machine readable
* it's intuitive enough for us human, to understand what the information means

note that it isn't exactly the JavaScript object syntax is
* JSON and JavaScript object are different
* if it was JavaScript object, the syntax is slightly different
	* we don't strictly need the quotation marks around the keys
```js
let data = {
	origin: "New York",
	destination: "London",
	duration: 415
}
```

but JavaScript knows how to turn a JSON to a JavaScript object
* Python as well knows how to turn a JSON to a dictionary

another nice thing about JSON representation is that,
it is very conducive to representing strucutres of things
* values don't have to be types like string or number, they can be lists or arrays as well
* or they can even be other JavaScript object

```json
{
	"origin": {
		"city": "New York",
		"code": "JFK"
	},
	"destination": {
		"city": "London",
		"code": "LHR"
	},
	"duration": 415
}
```

the important thing is,
as long as the JSON's provider and the receiver agree upon the representation, they both know what the names mean and what the structure of this JSON payload is, then the person on the receiving end and take the data and write a program that's able to interpret it and use it

eg. since currency exchange rate is ever changing, we probably need to use an API from an online service that keeping track of the exchange rate, rather than updating the exchange rate in our application by ourselves
```json
{
	"rates": {
		"EUR": 0.907,
		"JPY": 109.716,
		"GBP": 0.766,
		"AUD": 1.479
	},
	"base": "USD"
}
```
* that is a JSON from https://api.exchangeratesapi.io/latest?base=USD
	* all we need to gain this exchange rate information, is just making an HTTP request to that particular URL
___

# Ajax

thus far the JavaScript we wrote are all happening in the client's computer only
they don't communicate with a server of any kind

if we need that kind of behavior, we need something known as Ajax
* stands for *asynchronous JavaScript*
* the idea is that, we can make additional web requests to ask for additional information, from the Internet, including server from us or some third party web servers

`currency.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Currency Exchange</title>
		<script>
			document.addEventListener('DOMContentLoaded', function() {
				fetch('https://api.exchangeratesapi.io/latest?base=USD')
				.then(response => response.json())
				.then(data => {
					// console.log(data);
					const rate = data.rates.EUR;
					document.querySelector('body').innerHTML = `1 USD is equal to ${rate.toFixed(3)} EUR.`;
				});
			});
		</script>
	</head>
	<body>
	</body>
</html>
```

`fetch()`
* this function is built-in to more recent versions of JavaScript and supported by most major browsers now
* it will make a web request
	* it queries some webiste, and get back some HTTP response
	* if it was a third party server, we probably need to read its documentation to learn what API to use and what is the structure of the data that get back to us
* it returns a *promise*
	* it refers to the idea that, something is going to come back, but it may not be immediate
	* `fetch().then()` is how we define what it should do when it get back the response
		* in this case we will convert the response to a raw JSON data that we can use
		* then `fetch().then().then()` happens when the response is done converting to JSON

`rate.toFixed(3)`
* round `rate` to three decimal places
___

## eg. Exchange rate
here we improve the above example a little

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Currency Exchange</title>
		<script>
			document.addEventListener('DOMContentLoaded', function() {
				document.querySelector('form').onsubmit = function() {
					fetch('https://api.exchangeratesapi.io/latest?base=USD')
					.then(response => response.json())
					.then(data => {
						const currency = document.querySelector(#currency).value.toUpperCase();
						const rate = data.rates[currency];
						if (rate !== undefined) {
							document.querySelector('#result').innerHTML = `1 USD is equal to ${rate.toFixed(3)} ${currency}.`;
						} else {
							document.querySelector('#result').innerHTML = 'Invalid currency.';
						}
					})
					.catch(error => {
						console.log('Error:' + error);
					});
					
					return false;
				};
			});
		</script>
	</head>
	<body>
		<form>
			<input id="currency" placeholder="Currency" type="text">
			<input type="submit" value="Convert">
		</form>
		<div id="result">
		</div>
	</body>
</html>
```

`data.rates[currency]`
* note that we can't use `data.rates.currency` here because it would be trying to access the `currency` property, but instead we want to access the currency property that the user asked for
	* so in practice it would be something like `data.rates["EUR"]` or the likes

* one problem here is that the `currency` get substituted here, can be invalid
	* in that case, we will get back `undefined`
	* if we try to access a property of an object that doesn't exist, we would get back a particular JavaScript variable, `undefined`, meaning there is no such value
		* slightly different from `null`

`.catch(error => { })`
* whenever we use `fetch()`, we should have a `catch()` clause for it
	* because, you never know, maybe the API went down, maybe the JSON that it returns is changed so that our code can't read the JSON well
* we add this `catch()` clause to define, what it should do when something went wrong
	* when any thing about the `fetch()` and the `.then()` went wrong
___
