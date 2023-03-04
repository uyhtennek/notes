you might be realizing, at this point, application is starting to get fairly complicated
* there's a lot of JavaScript mess
* and as we want to start making things more interactive and dynamic and interesting, there's gonna be a lot of JavaScript code required

Therefore, in recent years, there are JavaScript libraries or frameworks that get developed
* allowing to more effeciently and effectively, create user interfaces
___

# React
one of the most popular JavaScript library

it enables us to design user interfaces that are very interactive
* because the content of the web page can update automatically, based on some underlying state

React is ultimately based on the idea of *declarative programming*
* the programming we are familiar with, the more classical programming styles, is a kind of *imperative programming*
	* we generally give the computer commands

let's say we have a *view* like this
```html
<h1>0</h1>
```

now we want to increment it by 1, our *logic* would be something as such
```js
let num = parseInt(document.querySelector('h1').innerHTML);
num += 1;
document.querySelector('h1').innerHTML = num;
```

which is quite some amount of code, for a fairly simple task
* that's due to we have to be very explicit about the instructions we're giving to the web browser

then the declarative programming approach, is just describing what state should be displayed and in what form
* its syntax is something like the following

```html
<h1>{num}</h1>
```

```js
num += 1;
```

> [!info] React
> Ultimately, what it is doing is that, it divides an application into a whole bunch of different components, where a component is things like the View in our example.
> 
> Then, based on some underlying state or variables, maybe we manipulated it, React will handle the process of updating what the user can see, for us.

___

# Implementing React

To get React working, we need to include at least three JavaScript packages inside of our web page
* React
	* allow us to define what are these components and how they behave
* ReactDOM
	* allow us to take React components and insert them into the DOM
* Babel
	* we use it to translate code from one language to another
	* because when we're writing React code, it isn't JavaScript, but JSX
		* which is an extension to JavaScript
		* it looks a lot like JavaScript, but has some additional features
			* in particular, it allows us to represent HTML inside of our JavaScript code in a way that it is much easier to read and use
	* but browsers don't understand JSX automatically, so we need Babel to translate JSX into JavaScript which our browsers understand

`react.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<script src="https://unpkg.com/react@17/umd/react.production.min.js"></script>
		<script src="https://unpkg.com/react-dom@17/umd/ract-dom.production.min.js"></script>
		<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
		<title>React</title>
	</head>
	<body>
		<div id="app"></div>
		
		<script type="text/babel">
			function App() {
				return (
					<div>Hello, world!</div>
				);
			}
			
			ReactDOM.render(<App />, document.querySelector("#app"));
		</script>
	</body>
</html>
```

`<div id="app">`
* it's React's job to fill things into it

`<script type="text/babel">`
* it tells the browser to translate this script before executing it
* because the code here is JSX but not JavaScript
* in practice, we would want to do this translation ahead of time though, so prior to deploying the application
	* here we just gonna translate it on the fly

All React applications are composed of components
* a component is just some part of the web application's user interface
* we describe a component witha JavaScript function
	* in our case, the `App` function
* the function returns what should appear inside of that component

`ReactDOM.render()`
* this is used to render React components into the page
* first argument is what component to render
* second argument is where on the page the component get rendered

and because it's JavaScript, we can have JavaScript code in the `App` function
```html
<script type="text/babel">
	function App() {
		const x = 1;
		const y = 2;
		return (
			<div>{ x + y }</div>
		);
	}
	ReactDOM.render(<App />, document.querySelector("#app"));
</script>
```
___

# Reusing Components
so where React gets more powerful, is we can begin to reuse components

eg. we want to display `<h1>Hello!</h1>` three times, so we do
```html
<script type="text/babel">
	function App() {
		return (
			<div>
				<h1>Hello!</h1>
				<h1>Hello!</h1>
				<h1>Hello!</h1>
			</div>
		);
	}
	ReactDOM.render(<App />, document.querySelector("#app"));
</script>
```
* but we are repeating ourselves here
* so maybe we should make `<h1>Hello!</h1>` a component of its own

```html
<script type="text/babel">
	function Hello() {
		return (
			<h1>Hello!</h1>
		);
	}
	
	function App() {
		return (
			<div>
			   <Hello />
			   <Hello />
			   <Hello />
			</div>
		);
	}
	
	ReactDOM.render(<App />, document.querySelector("#app"));
</script>
```

it doesn't seem much useful, but then we can parameterize those components
* with properties
* React simplifies them, as *props*

like how HTML elements can take attributes,
React components can take properties

```html
<script type="text/babel">
	function Hello(props) {
		return (
			<h1>Hello, {props.name}!</h1>
		);
	}
	
	function App() {
		return (
			<div>
			   <Hello name="Harry" />
			   <Hello name="Ron" />
			   <Hello name="Hermione" />
			</div>
		);
	}
	
	ReactDOM.render(<App />, document.querySelector("#app"));
</script>
```
___

# State
next, we can add states to components

first of all, *state* means any kind of data that we want to store inside of the component itself
* eg. we will recreate the [[1 - JavaScript and DOM#Adding Event Listener|counter app]] for demonstrating this
	* where we have a button that will increment a counter on clicked

we start off basically copying from the last example
* we re-design the page a bit

`counter.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<script src="https://unpkg.com/react@17/umd/react.production.min.js"></script>
		<script src="https://unpkg.com/react-dom@17/umd/ract-dom.production.min.js"></script>
		<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
		<title>Counter</title>
	</head>
	<body>
		<div id="app"></div>
		
		<script type="text/babel">
			function App() {
				return (
					<div>
						<div>0</div>
						<button>Count</button>
					</div>
				);
			}
			
			ReactDOM.render(<App />, document.querySelector("#app"));
		</script>
	</body>
</html>
```

now, first thing, it's not always gonna be `0`
* so we should factor that out, into what's known as state
* we have a special function for that inside of `React`, called `useState`

```html
<script type="text/babel">
	function App() {
		const [count, setCount] = React.useState(0);
		
		return (
			<div>
				<div>{count}</div>
				<button>Count</button>
			</div>
		);
	}
	
	ReactDOM.render(<App />, document.querySelector("#app"));
</script>
```

`React.useState(0)`
* this is one example of a React hook, that allows us to add some additional functionality into our React component
* the argument to it is going to be the intial value of the state
* it returns an array of two things
	* a variable
		* we can give it any name
	* a function to set the value of the variable, or set the value for the state
		* likewise, we can give it any name

next thing, we make the button does something
```html
<script type="text/babel">
	function App() {
		const [count, setCount] = React.useState(0);
		
		function updateCount() {
			setCount(count + 1);
		}
		
		return (
			<div>
				<div>{count}</div>
				<button onClick={updateCount}>Count</button>
			</div>
		);
	}
	
	ReactDOM.render(<App />, document.querySelector("#app"));
</script>
```

`<button onClick={updateCount}>`
* notice, traditionally in HTML, we would use `onclick` with a lower-case c, but here we used `onClick` with a capital C
	* it's just a common React convention
* we would define the `updateCount` function inside of our component, or the `App` function
	* note that in JavaScript we can have functions inside of a function

`setCount(count + 1)`
* you might think we can do `count = count + 1;` or something similar
	* it turns out, in React, we can't
* for changing a component's state, we have to use the function that `useState` provided
	* because you can imagine if we change the `count` directly, although it changes the state but it has no effect on the view
	* it demands us to use the function to change the state because it would also update the view
___

# eg. math quiz app

again, we start off copying from some previous example
* because the HTML structure is pretty much the same
* but re-design a bit

`addition.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<script src="https://unpkg.com/react@17/umd/react.production.min.js"></script>
		<script src="https://unpkg.com/react-dom@17/umd/ract-dom.production.min.js"></script>
		<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
		<title>Addition</title>
	</head>
	<body>
		<div id="app"></div>
		
		<script type="text/babel">
			function App() {
				return (
					<div>
						<div>1 + 2</div>
						<input />
					</div>
				);
			}
			
			ReactDOM.render(<App />, document.querySelector("#app"));
		</script>
	</body>
</html>
```

then, we factor out the hard coded content
```html
<script type="text/babel">
	function App() {
		
		const [state, setState] = React.useState({
			num1: 1,
			num2: 2
		});
		
		return (
			<div>
				<div>{state.num1} + {state.num2}</div>
				<input />
			</div>
		);
	}
	
	ReactDOM.render(<App />, document.querySelector("#app"));
</script>
```

`const = [state, setState] = React.useState({...});`
* a common practice in React is to combine multiple states into one JavaScript object
	* because as we add more states to a component, it can quickly get messy
* that JavaScript object is going to maintain all of the different pieces of state for the component

but then, we need some way to check if the user answers correctly or not
* so the user's answer is one thing that we need to keep track of in the `state`
	* which is the `response`
```html
<script type="text/babel">
	function App() {
		
		const [state, setState] = React.useState({
			num1: 1,
			num2: 2
			response: ""
		});
		
		function updateResponse(event) {
			setState({
				...state,
				response: event.target.value
			});
		}
		
		return (
			<div>
				<div>{state.num1} + {state.num2}</div>
				<input onChange={updateResponse} value={state.response} />
			</div>
		);
	}
	
	ReactDOM.render(<App />, document.querySelector("#app"));
</script>
```

`<input onChange={updateResponse} value={state.response} />`
* since the `<input>` now just gonna display `{state.response}`, we need to update `state.response` to make it display what we've typed
	* otherwise it is just gonna be blank no matter what we typed
* that is why we need `onChnage={updateResponse}`
	* `updateResponse` is just a function name, we can call it whatever we like

`event.target.value`
* in this context, it gets what the user typed into the input field

`...state`
* we actually need to update all the variables in `state`, when ever we use `setState`
	* so actually we need to say
```jsx
function updateResponse(event) {
	setState({
		num1: state.num1,
		num2: state.num2,
		response: event.target.value
	});
}
```
* and you can see this would be one painful thing to do if there are many different variables
* so there is a shorthand for this, which is `...state`
	* it means "just use the existing values for everything else"

last thing, we check the answer when the user press the Enter
* additionally we keep score of how many times the user answers correctly
```html
<script type="text/babel">
	function App() {
		
		const [state, setState] = React.useState({
			num1: 1,
			num2: 2
			response: "",
			score: 0
		});
		
		function updateResponse(event) {
			setState({
				...state,
				response: event.target.value
			});
		}
		
		function inputKeyPress(event) {
			if (event.key === "Enter") {
				const answer = parseInt(state.response);
				if (state.num1 + state.num2 === answer) {
					// user answers correct
					setState({
						...state,
						score: state.score + 1
					});
				} else {
					// user answers wrong
					setState({
						...state,
						score: state.score - 1
					});
				}
			}
		}
		
		return (
			<div>
				<div>{state.num1} + {state.num2}</div>
				<input autoFocus={true} onKeyPress={inputKeyPress} onChange={updateResponse} value={state.response} />
				<div>Score: {state.score}</div>
			</div>
		);
	}
	
	ReactDOM.render(<App />, document.querySelector("#app"));
</script>
```

`onKeyPress={inputKeyPress}`
* get triggered any time a user pressed a key, any key

`autoFocus={true}`
* just enabling auto-focus

and the last thing, since the question never change, let's fix that
* we want to clear the user's input as well
```jsx
function inputKeyPress(event) {
	if (event.key === "Enter") {
		const answer = parseInt(state.response);
		if (state.num1 + state.num2 === answer) {
			// user answers correct
			setState({
				...state,
				num1: Math.ceil(Math.random()) * 10,
				num2: Math.ceil(Math.random()) * 10,
				score: state.score + 1,
				response: ""
			});
		} else {
			// user answers wrong
			setState({
				...state,
				score: state.score - 1,
				response: ""
			});
		}
	}
}
```

and let's make the page prettier by introducing some CSS
and let the user win the game when they get to a certain score
```html
<style>
	#app {
		text-align: center;
		font-family: sans-serif;
	}
	
	#problem {
		font-size: 72px;
	}
	
	.incorrect {
		color: red;
	}
	
	#winner {
		font-size: 72px;
		color: green;
	}
</style>
...
<script type="text/babel">
	function App() {
		
		const [state, setState] = React.useState({
			num1: 1,
			num2: 2
			response: "",
			score: 0,
			incorrect: false
		});
		
		function updateResponse(event) {
			setState({
				...state,
				response: event.target.value
			});
		}
		
		function inputKeyPress(event) {
			if (event.key === "Enter") {
				const answer = parseInt(state.response);
				if (state.num1 + state.num2 === answer) {
					// user answers correct
					setState({
						...state,
						score: state.score + 1,
						incorrect: false
					});
				} else {
					// user answers wrong
					setState({
						...state,
						score: state.score - 1,
						incorrect: true
					});
				}
			}
		}
		
		if (state.score === 10) {
			return (
				<div id="winner">
					You Won!
				</div>
			);
		}
		
		return (
			<div>
				<div className={state.incorrect ? "incorrect" : ""} id="problem">{state.num1} + {state.num2}</div>
				<input autoFocus={true} onKeyPress={inputKeyPress} onChange={updateResponse} value={state.response} />
				<div>Score: {state.score}</div>
			</div>
		);
	}
	
	ReactDOM.render(<App />, document.querySelector("#app"));
</script>
```

since any React component is a function, we actually can add logic to it
* in our previous examples it simply return some view
* this time we want it to check something first and return different view accordingly
	* so we can change the view completely, very easily
___

# Other Libraries
or web frameworks

* Angular
* Vue

> [!tip] The world is changing
> The world of user interfaces is changing pretty quickly. There are a lot of changes in user interfaces in terms of the technologies and the tools that are quite popular. But at least they're based on the same set of underlying ideas. Which mean, the JavaScript event listener parts never change...

___
