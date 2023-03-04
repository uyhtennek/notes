# CSS Animation

To make HTML elements more interesting, we can add animations to them
* so elements can move around, and change properties in some way, over time

That is something CSS can do
* CSS has already given us support, for styling elements
* it also gives us the ability to animate those properties as well

`animate.html`
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Animate</title>
		<style>
			@keyframes move {
				from {
					/* the initial properties, or the starting keyframe */
					left: 0%;
				}
				to {
					/* the ending keyframe */
					left: 50%;
				}
			}
			
			h1 {
				position: relative;
				animation-name: move;
				animation-duration: 2s;
				animation-fill-mode: forwards;
			}
		</style>
	</head>
	<body>
		<h1>Welcome!</h1>
	</body>
</html>
```

for any animation that we want an element to play, it needs
* the name of the animation, `animation-name`
* a duration, `animation-duration`
* `animation-fill-mode`
	* it defines in what direction should the animation move
___

## Keyframes
as its name implies, surely an animation has more than just the start and the end

```html
<style>
	@keyframes move {
		0% {
			left: 0%;
		}
		50% {
			left: 50%;
		}
		100% {
			left: 0%;
		}
	}
	
	h1 {
		position: relative;
		animation-name: move;
		animation-duration: 2s;
		animation-fill-mode: forwards;
		/* we can make the animation play twice or more */
		animation-iteration-count: 2;
	}
</style>
```

`animation-iteration-count`
* we can tell an animation to play not just once, but more
* we can do `animation-iteration-count: infinite;` to make it play infinitely
___

# Animate with JavaScript
and we can use JavaScript to control animations

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Animate</title>
		<style>...</style>
		<script>
			document.addEventListener('DOMContentLoaded', function() {
				const h1 = document.querySelector('h1');
				// by default, so when starting out, the animation is paused
				h1.style.animationPlayState = 'paused';
				
				document.querySelector('button').onclick = () => {
					if (h1.style.animationPlayState === 'paused') {
						h1.style.animationPlayState = 'running';
					} else {
						h1.style.animationPlayState = 'paused';
					}
				};
			});
		</script>
	</head>
	<body>
		<button>Click Here!</button>
		<h1>Welcome!</h1>
	</body>
</html>
```

`h1.style.animationPlayState`
* `animationPlayState` is a property of `style` of any element
* it decides whether the animation is playing or paused
* and we can control that via JavaScript
___

# eg. Hide Elements

this example starting out, very similar to the [[1 - Dynamic Loading#infinite scroll|infinite scroll example]]
* except there is a Hide button for each post

`index.html`
```html
<script>
	...
	function add_post(contents) {
		// Create new post
		const post = document.createElement('div');
		post.className = 'post';
		post.innerHTML = `${contents} <button class="hide">Hide</button>`;
		
		// Add post to DOM
		document.querySelector('#posts').append(post);
	}
</script>
```

the first thing we want to do is to,
detect when the user clicks on one of those Hide buttons
* there are multiple ways to this
* this time we will start from detecting if the `document` itself is clicked, rather than doing detection for each button, just for demonstration
	* then we can ask, "what did they actually click on?"

```html
<script>
	document.addEventListener('click', event => {
		const element = event.target;
		if (element.className === 'hide') {
			element.remove();
		}
	});
</script>
```

it turns out, most event listeners, have an optional argument, which is the `event` itself
* which is a JavaScript object that contains information about the event

`event.target`
* what was the target of the `event`?
* in this case, what was the thing that was actually clicked on?

if we run the server and test the code out, it doesn't quite work
* the element that gets removed is the Hide button that gets clicked on
* instead of the `<div>` element which is mimicking a social media post

fortunately it's an easy fix, we just need to remove the parent of the element that get clicked on
* because the parent of the Hide button is the post
* so we change `element.remove()` to `element.parentElement.remove()`

now, although it works, it isn't obvious what is going on to the eye
* because the posts all have the same height and same appearance, at least for this example
* so this can be a time where animation can be quite helpful

```html
<style>
	@keyframes hide {
		0% {
			opacity: 1;
		}
		100% {
			opacity: 0;
		}
	}
	
	.post {
		... /* some other normal style */
		animation-name: hide;
		animation-duration: 2s;
		animation-fill-mode: forwards;
		animation-play-state: paused;
	}
</style>
<script>
	document.addEventListener('click', event => {
		const element = event.target;
		if (element.className === 'hide') {
			element.parentElement.style.animationPlayState = 'running';
			element.parentElement.addEventListener('animationend', () => {
				element.parentElement.remove();
			});
		}
	});
</script>
```

`element.parentElement.addEventListener('animationend', () => { })`
* as you can imagine, `'animationend'` is an event happens when an animation ends

now it is starting to get more interesting to see
but still, it doesn't look quite right that the post jumps up abruptly right when the post disappear, so let's fix that

```html
<style>
	@keyframes hide {
		0% {
			opacity: 1;
			height: 100%;
			line-height: 100%;
			padding: 20px;
			margin-bottom: 10px;
		}
		75% {
			opacity: 0;
			height: 100%;
			line-height: 100%;
			padding: 20px;
			margin-bottom: 10px;
		}
		100% {
			opacity: 0;
			height: 0px;
			line-height: 0px;
			padding: 0px;
			margin-bottom: 0px;
		}
	}
</style>
```

basically, `height`, `line-height`, `padding`, and `margin` are the attributes that can affect an element's height
___
