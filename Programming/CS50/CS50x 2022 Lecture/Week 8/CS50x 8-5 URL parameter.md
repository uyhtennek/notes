# URL parameter

actually there is yet another format of [[CS50x 8-2 Request and Response#URL|URL]]
which look something like `https://www.example.com/path?key=value`

you might not notice but this `key=value` thing actually is everywhere
* what ever we typed into a search engine like Google, ends up in the URL
* when we click on a link, there might be a question mark `?` and some keys and values
	* there might also be an ampersand `&` and then more keys and values


1. Get input
just try searching anything on Google, for example `cats`
the URL would change from `https://www.google.com` to something like
* `https://www.google.com/search?q=cats&...`
* it would also work if we just throw away the `&...` part

The point is that, the URL changed to include the user's input
* this is how humans provide input to servers
* also the the humans don't have to manually type out the new URL, it's auto-generated

When we fill out a form on the web, and hit Enter
* typically the URL suddenly changes
	* to include what ever we typed in


2. Request
* assuming the form is using the verb `GET`
	* but that's not ideal right?
	* if we typed the username, password, credit card information, we wouldn't like the idea of that they are shown in the URL
	* if so the next person sit down to use our laptop, can see everything that we typed in, just by looking at the history
* so there is another verb `POST`, that can hide all of that

but for now, back to the `GET` version first
recall how would the [[CS50x 8-2 Request and Response#Request and Response|HTTP header]] look like, if search the word `cats` using Google?
```
GET /search?q=cats HTTP/1.1
Host: www.google.com
...
```

then hopefully what comes back to us, is a page full of search results, with `cats` being the thing to search for
___

# Implementing a search feature

Now let's look at some tech stuff.
Think back about Google, or searching something using Google.

Why and how the things that we typed in the search bar, get automatically placed at the URL as the URL parameter?
* It doesn't need us to manually type `https://www.google.com/search?q=cats` to search something
* All we did is typed `cats` in the search bar and hit Enter
* Then the URL `https://www.google.com/search?q=cats` is generated

Perhaps the HTML looks something similar than this
```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <title>search</title>
    </head>
    <body>
		<form action="https://www.google.com/search" method="get">
			<input name="q" type="text">
			<input type="submit">
		</form>
    </body>
</html>
```

`<input name="q" type="text">`
* `name="q"` where `"q"` represents *query*, that query that the human's typing in
	* Larry and Sergey of Google back in the '90s decided that when they created Google.com
* `type="text"`, meanings put a text box / input field here in this input tag
* there are some more properties that we can play around too
	* try `<input autocomplete="off" autofocus name="q" placeholder="Query" type="text">`
	* `autofocus` to automatically put the cursor instead of the text box

`<form action="https://www.google.com/search" method="get">`
* `method="get"`
	* actually by default it would use `get` anyway
	* notice it's all lower case in here, although the `GET` is the headers are in fact upper case, by convention
* `action="https://www.google.com/search"`
	* here we just send the user from our web page to the Google search engine
	* since we don't have a search engine of our own

* The whole thing means we're creating a form, the action of which is to send the data to `https://www.google.com/search`, using the `get` method
	* it's gonna send an input called `"q"`
	* whenever the user click the Submit buton
		* which is created by the `<input type="submit">` tag

> [!info]
> Here we basically implemented the **front end** of google.com (surely the actual google site would look much prettier than ours). But not the back end.
> The **back end** would be a really big database, some SQL too to do some queries, and Python for automate the queries.

___
