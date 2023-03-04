# JavaScript vs Ajax

before, our JavaScript is rather simple. it can only do something like
* push a button and something happens
* the catch is, everything is client side
	* all happening in our computer
	* we never let it talk to the outside world

Ajax
* formerly *Asynchronous JavaScript and XML*
	* although this is not really what it's called anymore
* actually, most commonly now, instead of XML, we use JSON
	* Ajax is the name that's kind of stuck there for the technique
* it essentially allows us to refresh sections of our page, but not the entire page

eg. sports website
* scores of games that are going on currently are being updated
* but the whole page doesn't refresh
* that's what Ajax does, just updating small portions of the page

this time we too are making a button. When it's clicked something happen on our web site.
* the catch is, before it's just happening on our machine
* this time we're making a server request, an outbound request from a page
	* if we want to do things like updating the scores on a sports website, or updating the inbox of Gmail, etc.
	* the technique is the same. it's Ajax.
	* but in those cases it's some code running constantly all the time, querying forever
___

# XMLHttpRequest

for that, we need a special kind of JavaScript object called an **XMLHttpRequest**
* it allows us to make a asynchronous request to a server to get some information
	* *asynchronous* means not at the same time
	* so it's not at the same time as the refresh of the page or loading the page, but sometime thereafter while we still are on the page

it's easy to do so fortunately
```js
var xhttp = new XMLHttpRequest();
```

* it creates a new x amount of HTTP request object
	* in this example we would call it `xhttp`

then sure enough, it's not done
the first thing to do is we need to define its `onreadystatechange` behavior
* `onreadystatechange` = *on ready state change*

* that's to say, the steps that are happening when we visit a page
	* loading a page or refreshing a page, actually is going through a series of different states
		* the request hasn't been initiated; we initiated the request; we sended the request; we received the first response; we received all the responses; the request is completed.
		* those are just examples of a couple of different state changes that might take place
	* although we're just going to update a smaller section of the site, some or all the steps or state changes are still gonna take place

* typically, we define something that is supposed to happen when the state change, using an anonymous function

there are 5 different states
* `0`, `1`, `2`, `3`, `4`
* they are the `readyState` property
	* part of the `onreadystatechange`
* `0` - request not yet initialized
* `4` - the entire thing has loaded. request finished, response ready.

`status` is another property of `onreadystatechange`
* hopefully it's `200 OK`

and we do care about `readyState` and `status`
we want to make sure that when our Ajax request is completed,
* the `readyState` is `4`, so the data has been received
* the `status` is `200 OK`, everything works fine

if both are the case, the web page is successfully loaded,
we are ready to update our site, by sending Ajax request
* for that we need to `open()` the request
* and `send()` the request
* then our site will refresh

one more thing, later we will do it using pure JavaScript
* just so we see it
but it's common to use jQuery to handle Ajax requests
* for jQuery is popular for client-side scripting
	* eg. DOM manipulation, Ajax request
___

# Ajax in code

here is a JavaScript function that is preparing, opening, and sending an Ajax request
```js
function ajax_request(argument)
{
	var aj = new XMLHttpRequest();
	aj.onreadystatechange = function() {
		if (aj.readyState == 4 && aj.status == 200)
			// do something to the page
	};
	
	aj.open("GET", /* url */, true);
	aj.send();
}
```

1. `var aj = new XMLHttpRequest();`
* first thing is creating a new `XMLHttpRequest`

2. `aj.onreadystatechange = function() {};`
* here we defined a function that is going to execute on the `readyState` changing
	* every time the `readyState` changes, the function will execute
* although the function will run every time `readyState` changes, the time this function is actually meaningful is when `aj.readyState` is `4` and `ad.status` is `200`

3. `aj.open("GET", /* url */, true);`
* by this line we defined the behavior of our `XMLHttpRequest` variable, `aj`
* now we `open()` up our XML request or Ajax request
* it's a `GET` request to a particular URL
	* don't worry about what the `true` means

4. `aj.send();`
* finally send that request

extra.
it's common to use jQuery instead of pure JavaScript for writing Ajax requests
see http://api.jquery.com/jquery.ajax/
___

# eg. Ajax Test

```html
...
	<head>
		<title>Ajax</title>
		<script src="https://code.jquery.com/jquery-1.11.0.min.js" type="text/javascript"></script>
		<script src="js/ajax.js"></script>
		<link href="css/div.css" rel="stylesheet" />
	</head>
	<body>
		<h2>Ajax Test</h2>
		<div id="infodiv">
			Information goes here
		</div>
		<div>
			<form>
				<select name="staff" onchange="cs50Info(this.value);">
					<option value="">Select someone:</option>
					<option value="bowden">Rob Bowden</option>
					<option value="chan">Zamyla Chan</option>
					<option value="malan">David J. Malan</option>
					<option value="zlatkova">Maria Zlatkova</option>
				</select>
			</form>
		</div>
	</body>
...
```

1. this time we use a little bit of jQuery
* just to show how it works with jQuery

2. `<div id="infodiv">` is the thing we'll be updating

3. `<select name="staff" onchange="cs50Info(this.value);">`
* when the value of this drop down menu changes, when the user select a different options from the menu, it calls a function, `cs50Info()`
* `this.value` is its parameter
	* recall `this` is a way to self-refer to the event that triggered the function
		* refer to the caller of the function
	* so `this.value` indicates the `value` the user selected
		* `value` in the `<option>`
		* `""`, `"bowden"`, `"chan"`, `"malan"`, `"zlatkova"`
* another example of event handler like `onchange`, is `onclick`

so what does `cs50Info()` do?
let's take a look at `ajax.js`
```js
function cs50Info(name)
{
	// when nothing is chosen
	if (name == "")
		return;
	
	// create a new AJAX object
	var ajax = new XMLHttpRequest();
	
	// fill the <div id="infodiv"> with some HTML
	// as the response to our ajax request that the server gave us
	ajax.onreadystatechange = function() {
		if (ajax.readyState == 4 && ajax.status == 200) {
			$('#infodiv').html(ajax.responseText);
		}
	};
	
	// open the requested file
	ajax.open('GET', name + '.html', true);
	// and transmit the data
	ajax.send();
}
```

notice we don't refresh the whole page,
but only just the `<div id="infodiv">` section

what the `name + .html` look like
are just some snippets of HTML
* not a complete and correct HTML file
	* don't have `<html>`, `<head>`, `<body>`, etc.
* but it's some HTML there

```html
<p>Zamyla Chan</p>
<img src="img/chan.jpg" />
<p>Class of 2014</p>
<p>Winthrop House</p>
```

finally when we see it in action
* we might see very quickly the content of `<div id="infodiv">` get deleted and placed back
* that is the asynchoronous request happening very quickly
* again the web page never refresh, it's just the `<div id="infodiv">` section, that we care about

* one last thing is that, our Ajax request doesn't have to be sent to servers that are running locally
	* maybe we can sourcing information from Yahoo Finance
	* and load their information to our page
* even cooler is that we can have some way to constantly update the information
___
