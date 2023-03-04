here we are talking about user interface design
___

# Single-Page Applications

we developed web applications that have multiple different pages
* they are done via multiple different routes
	* eg. different paths in a Django web application

but single-page applications are quite common
* the entire web application is just a single page
* then we use JavaScript to manipulate the DOM to display different information

but why we would want this?
* we only need to make modifications to the part of the page that is actually changing
	* if we have five different but similar pages, why loading the entire page, rather than loading only the different parts?
* it is especially helpful for applications that are changing quite frequently

eg. we want to make a single-page application that displays three different pages
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Single Page</title>
		<style>
			div {
				/* by default, hide the div */
				display: none;
			}
		</style>
		<script>
			function showPage(page) {
				// hide all div
				document.querySelectorAll('div').forEach(div => {
					div.style.display = 'none';
				});
				// show only the correct div
				document.querySelector(`#${page}`).style.display = 'block';
			}
			
			document.addEventListener('DOMContentLoaded', function() {
				document.querySelectorAll('button').forEach(button => {
					button.onclick = function() {
						showPage(this.dataset.page);
					};
				});
			});
		</script>
	</head>
	<body>
		<button data-page="1">Page 1</button>
		<button data-page="2">Page 2</button>
		<button data-page="3">Page 3</button>
		<div id="page1">
			<h1>This is page 1.</h1>
		</div>
		<div id="page2">
			<h1>This is page 2.</h1>
		</div>
		<div id="page3">
			<h1>This is page 3.</h1>
		</div>
	</body>
</html>
```

note that it is mimicking multiple pages, but it doesn't require making any request to a server in order to get access to another page's information
* although sometimes we do need to reach out to the server

another problem is that, you might imagine it would be inefficient
* we are loading all three pages immediately and just showing and hiding them when we need to
* but maybe the user just care about the first page but not the others
	* they are never going to look at page 2 and page 3, yet we load them, that's inefficient
* so, what we can do, is loading these data dynamically
	* so it only change the parts that need to be changed
	* and here we start to bring back Django

`urls.py`
```python
urlpatterns = [
	path("", views.index, name="index"),
	path("sections/<int:num>", views.section, name="section")
]
```
the `section/<int:num>` route here is for dynamically loading different sections of a page

`views.py`
```python
def index(request):
	return render(request, "singlepage/index.html")

def section(request, num):
	if 1 <= num <= 3:
		return HttpResponse(texts[num - 1])
	else:
		raise Http404("No such section")
```
`texts` is just a list of 3 str

`templates/singlepage/index.html`
```html
<!DOCTYPE html>
<html lang="en">
...
	<script>
		function showSection(section) {
			fetch(`/sections/${section}`)
			.then(response => response.text())
			.then(text => {
				console.log(text);
				document.querySelector('#content').innerHTML = text;
			});
		}
		
		document.addEventListener('DOMContentLoaded', function() {
			document.querySelectorAll('button').forEach(button => {
				button.onclick = function() {
					const section = this.dataset.section;
					showSection(section);
				};
			});
		});
	</script>
...
	<body>
		<h1>Hello!</h1>
		<button data-section="1">Section 1</button>
		<button data-section="2">Section 2</button>
		<button data-section="3">Section 3</button>
		<div id="content">
		</div>
	</body>
</html>
```
___

## JavaScript History API

the downside is that, the URL no longer show where we are in the web application
* because we never leave the page at all

however, it turns out, in JavaScript, we have way to manipulate the URL
* taking advantage of what's known as the JavaScript history API
* we can push something to the history
	* meaning update the URL
	* and actually save the URL inside the user's browser history
* so later on users can potentially go back to that state of the web application

`templates/singlepage/index.html`
```html
<script>
	window.onpopstate = function(event) {
		console.log(event.state.section);
		showSection(event.state.section);
	}
	
	function showSection(section) {
		... // same as above
	}
	
	document.addEventListener('DOMContentLoaded', function() {
		document.querySelectorAll('button').forEach(button => {
			button.onclick = function() {
				const section = this.dataset.section;
				// this line is new
				history.pushState({section: section}, "", `section${section}`);
				showSection(section);
			};
		});
	});
</script>
```

``history.pushState({section: section}, "", `section${section}`)``
* basically, it adds a new elemnt to the browser's history 
* first argument is a JavaScript object, representing the state to push to the history
	* in this case we would store the section number in the history
* next argument is a title parameter, that most web browsers actually ignore, so we just put an empty string there
* `section${section}` is what should go in the URL

`window.onpopstate`
* this is the event that get triggered when we click the Back button in our web browser, to go back in history
* `onpopstate` refers to when we pop something off of the history
___

## Real-Life Example

there's certainly nothing wrong with our original paradigm
* of just loading different pages dynamically using Django
* making requests and getting responses

but often times, there can be a lot of things changing on the same page simultaneously
* eg. if we are on a social networking websites, there are a lot of things stay the same, but there also can be new posts get added
	* and we might be looking at different parts of the same page

therefore, to dynamically making requests and then display the additional information on the page, can be quite powerful
* in terms of making our web pages more interactive
___

# `window`

this JavaScript's `window`, refers to the browser window
* so it is the window in the user's Google Chrome or Safari or what ever browser they are using

it comes with a couple of useful properties
* `window.innerWidth`
	* how wide is the window?
* `window.innerHeight`
	* similarly, how high is the window?

and JavaScript also gives us `document`
* which generally represents the entire web page
* since, often times, web pages are long enough that it doesn't fit entirely inside of the window
	* users generally have to scroll through the web page to see different portion of the page
	* in other words, in these cases, the window can only show one portion of the page at a time

then it comes to this `window.scrollY`
* it represents how many pixels far down have you scrolled?
* if you're at the top of the page, `window.scrollY` is 0, because you haven't scrolled at all
* but as we begin to scroll, `window.scrollY` increases

and then the entire height of the page is represented by `document.body.offsetHeight`

![scroll.png (2014×1378) (harvard.edu)](https://cs50.harvard.edu/web/2020/notes/6/images/scroll.png)

we care about these properties because, we can now start doing some interesting calculations
* eg. has the user scrolled down to the bottom of the page?
	* `window.scrollY + window.innerHeight >= document.body.offsetHeight`

`scroll.html`
```html
<head>
	<title>Scroll</title>
	<script>
		window.onscroll = () => {
			if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
				// the user reached the end of the page, even passed it
				document.querySelector('body').style.background = 'green';
			} else {
				document.querySelector('body').style.background = 'white';
			}
		}
	</script>
</head>
<body>
	<p>1</p>
	<p>2</p>
	<p>3</p>
	....
	<p>100</p>
</body>
```

`window.onscroll`
* it listens for when the user scrolling through the window
___

## infinite scroll

although this might not be a practical use of detecting scroll, you can imagine in a social networking website, there is infinite scroll
* it has a bunch of posts, and if we scroll to the bottom of the list of posts, it generates the new set of posts
* or if it was a news website, in which is a bunch of news article
* so that the web page can get updated and feed to us new content, without us needing to load to a different page

how it can be done?
1. detect if the user scrolled to the bottom of the page
2. use Ajax to make request and get response
3. take the information and manipulate the DOM to update the web page

`urls.py`
```python
urlpatterns = [
	path("", views.index, name="index"),
	path("posts", views.posts, name="posts")
]
```

`views.py`
```python
def index(request):
	return render(request, "posts/index.html")

def posts(request):
	
	# Get start and end points
	start = int(request.GET.get("start") or 0)
	end = int(request.GET.get("end") or (start + 9))
	
	# Generate list of posts
	data = []
	for i in range(start, end + 1):
		data.append(f"Post #{i}")
	
	# Artificially delay speed of response
	time.sleep(1)
	
	# Return list of posts
	...
```

this `posts` route functions kinda like an API
* we go to the URL http://127.0.0.1:8000/posts?start=1&end=10
* it gives back to us a JSON

`templates/posts/index.html`
```html
<head>
	<script>
		// Start with first post
		let counter = 1;
		
		// Load posts 20 at a time
		const quantity = 20;
		
		// When DOM loads, render the first 20 posts
		document.addEventListener('DOMContentLoaded', load);
		
		window.onscroll = () => {
			if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
				// if the user has scrolled to the end of the page
				load();
			}
		}
		
		// Load next set of posts
		function load() {
			// Set start and end post numbers, and update counter
			const start = counter;
			const end = start + quantity - 1;
			counter = end + 1;
			
			// Get new posts and add posts
			fetch(`/posts?start=${start}&end=${end}`)
			.then(response => response.json())
			.then(data => {
				data.posts.forEach(add_post);
			});
		};
		
		// Add a new post with given contents to DOM
		function add_post(contents) {
			// Create new post
			const post = document.createElement('div');
			post.className = 'post';
			post.innerHTML = contents;
			
			// Add post to DOM
			document.querySelector('#posts').append(post);
		}
	</script>
</head>
<body>
	<div id="posts">
	</div>
</body>
```
___
