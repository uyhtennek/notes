[CS50 2022 - Seminar - Introduction to Node.js: Using Server-Side JavaScript](https://video.cs50.io/Qao10iN0lvU)
___

# What is Node.js?
* a *JavaScript runtime environment*
	* An environment to run javascript outside of a browser
* often used to build back-end services
* as well as APIs, or *Application Programming Interfaces*

## Pros
* best to build highly-scalable, data-intensive, real-time applications
* JavaScript, in both front-end and back-end
* many open-source libraries and packages
___

# Set-up

1. create `app.js` or `index.js`
* we can run `node app.js` to execute the program
	* as if it's a python file

2. `package.json`
* a universal file in Node.js that contains metadata about
	* the Node packages that we installed
	* the project name, description, and other details
* every node project has one
* run `npm init` to get one
	* then just press enter for everything if you don't care
___

# Create a server

in this example we'll use **Express**
* a minimal web application framework

run `npm install express`
* it'd give us another json file, `package-lock.json`
	* which is just a more specific metadata, a more detailed `package.json`
* if we look into `package.json`, *express* is now there

* there is a `node_modules/` folder as well
	* which we basically don't touch
	* it's all the stuffs that express installed
		* later if we install more other packages, `node_modules/` is gonna be filled with more stuffs

### initializing git
* don't include `node_modules/`
* but do include `package.json` and `package-lock.json`
	* because they tell what to include in `node_modules/`

## set-up `app.js`

```js
const express = require("express");

const app = express(); // run the Express app, put it in a variable `app`
const port = 3000;     // port to listen on

app.listen(port, () => {
	console.log(`CS50 app listening on port ${port}`);
});
```


## `nodemon`
it's a package for automating node server

### set-up
* run `npm install nodemon` to install
* add `"dev": "nodemon app.js"` in `package.json > "scripts"`
* run `npm run dev` to run the server

### pros
* if we haven't use it, when ever we made changes, we'd need to close the server, save the changes, and restart the server, to make those changes
* `nodemon` automatically restart the server when ever we make changes
___

# Routes in Express
general pattern: `app.<METHOD>(<PATH>, <HANDLER>)`

eg.
```js
app.get('/', (req, res) => {
	// req - request; res - response.
	res.send('Hello World!');
});
```

## templating engine
* we don't have Jinja for JavaScript, it's for Python
* we can find similar ones
	* we used Pug for this example
	* eg. EJS, Handlebars

## Pug

* run `npm install pug` to install Pug
* in `app.js`, we need to tell express that we're using Pug
	* by typing `app.set('view engine', 'pug')`

* create a file named `index.pug`
	* it's a common practice to place it in a `views/` directory
```pug
// file: index.pug
html
	head
		title= title
	body
		h1= message
```

* from our Express/Node server, we can feed to `index.pug` informations, if needed
```js
// file: app.js
...
app.get('/', (req, res) => {
	res.render('index', {
		title: 'Hey',
		message: 'Hello there!'
	});
	// ----
	// this instead just send the file, didn't use the templating engine
	res.sendFile('index.html')
});
```
___

# HTTP Request

1. firstly we need to set-up the request in the pug file
```pug
...
body
	h1= message
	h3 My to-dos
	form(id='index' action='/' method='post')
		input(name='todo' type='text' placeholder='Type todo here')
		input(type='submit' value='Submit')
```

2. then we need to make sure Express can read the request
```js
...
app.use(express.urlencoded({extended: true}));
app.use(express.json()); // Express read request in a JSON format
...
```

3. make the server supports that route
```js
...
app.post('/', (req, res) => {
	console.log(req.body);
	// if we don't give a response, the website will spin forever
	res.redirect('/'); // in this case we just redirect the user
});
...
```
___

# Database
we used SQLite3

* install `node-sqlite3` by running `npm install sqlite3`
	* so that we can do `sqlite3` stuffs in `app.js`
	* like `db.execute()` from the `cs50` library, for Flask's `app.py`
* [TryGhost/node-sqlite3: SQLite3 bindings for Node.js (github.com)](https://github.com/TryGhost/node-sqlite3)

now let's get our database ready
* just normal sqlite3 stuffs
```bash
sqlite3 todo.db
```
```sqlite
CREATE TABLE todos(name TEXT NOT NULL);
```

then we need to set-up `app.js` for interacting with the database, `todo.db`
```js
const sqlite3 = require('sqlite3').verbose();
const db = new sqlite3.Database('./todo.db');
```

and then the actual interaction
```js
...
app.post('/', (req, res) => {
	console.log(req.body);
	let todo = req.body.todo;
	db.run('INSERT INTO todos(name) VALUES (?)', todo);
	res.redirect('/');
});
...
```
___

# Final touch
let's finish the todo web app

```js
// file: app.js
...
app.get('/', (req, res) => {
	let todolist = [];
	db.each('SELECT name FROM todos', (err, row) => {
		if (err) {
			console.log(err);
		}
		else {
			console.log(row);
			todolist.push(row.name);
		}
	}, (err) => {
		return res.render('index', {
			title: 'Hey',
			message: 'Hello there!',
			todolist: todolist
		})
	});
});
...
```

`db.each()`
* for each rows from the result, do the things we specified

`(err, row) => { }`
* `err` - error; `row` - row.
* this is a function that will run for each row in the result

`(err) => { }`
* this is a second function, which will run when the query is finished

lastly the Pug file needs to read in the `todolist`
```pug
html
	head
		// ...
	body
		// ...
		for todo in todolist
			li= todo
		else
			p there are no todos...
```
___

# Recommended Alternatives

MongoDB vs SQLite
* SQLite is local, but MongoDB is in the cloud

try using frontend JavaScript library
* eg. React, Vue
* the professional ones
___
