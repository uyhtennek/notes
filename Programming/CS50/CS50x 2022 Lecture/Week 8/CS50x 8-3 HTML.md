# HTML
*Hypertext Markup Language*
a fundamental component of every website

it's actually not a programming languages
* a programming language is like C, Python or SQL
	* they have conditions and therefore we can express logic.
* HTML doesn't have variables, logic, functions, and the like
* HTML and CSS are not so much about logic
	* HTML is about structure
	* CSS is about the aesthetics of a page
* They are the skeleton of a web page
	* but to make it move and interactable, we need a proper programming language, for it allows logic

It's a *markup language*
* define structure of a web page
* causing the plain text inside of sets of tags to be interpreted by web browsers in different ways

Firstly, let's look at a `hello.html`
* it's a HTML page, not HTML program, that would be inaccurate
* HTML files' names typically ends with a .html extension
```html
<!DOCTYPE html>

<html lang="en">
	<head>
		<title>hello, title</title>
	</head>
	<body>
		hello, body
	</body>
</html>
```

* to see how it looks like we can open the file with a browser
* or in CS50 IDE we can right-click on the file and then select Preview

Sometimes, we may see a HTML file that doesn't do indentation or doesn't have any whitespace at all, very poorly styled. That is because whitespace is data, and data costs money.
* getting rid of whitespace turns out to be a good idea, cost-wise
* but since we're still learning stuffs and developing stuffs so it's better to keep the whitespace
* just remember that functionally they are identical
___

# `http-server`

That's it. We got our web page now.
But let's pause here, and think about how we could host this web page?

Especially, in the case of CS50, the html file is created through a remote VS Code environment, which itself is a website hosted already by a server
* the server is hosting a website already, how it could host another one?

1. Well, by default, any website, or VS Code actually, uses TCP port number `80` or `443`
* `80` for HTTP; `443` for HTTPS.
* In the case of CS50 IDE, maybe the VS Code website is running on port `443`

2. Then we just need to pick another port number for our own website to use
* it's like when our TCP packet arrives its destination, the cloud server
* the server just gives us different files, depending on the port number
* as a result we can run a web server, ours, on a web server, the CS50 IDE

3. We can do that in the CS50's VS Code
    by executing the `http-server` command in the terminal
* that's a command that CS50's folks pre-installed in the cloud that we're using
* a pop-up would come up telling that our application, web application or web server, is running on port `8080`
	* `8080` is a commonly used TCP port number, when `80` and `443` is already used

4. Then when we get to our own website
* we can see a *directory listing* of the web server, listening on port `8080`
* in which is our web page `hello.html`
	* but we won't see any files or anything that is in the other web server, listening on port `443`
___

# Tags and Attributes
HTML is characterized really by just two features
* tags and attributes
* tags are also known as *element*
	* more specifically element is the terminology that includes the start tag and the end tag and everything in between

let's bring back this again
```html
<!DOCTYPE html>

<html lang="en">
	<head>
		<title>hello, title</title>
	</head>
	<body>
		hello, body
	</body>
</html>
```

`<!DOCTYPE html>`
* This is the *Document Type declaration*
* It says to the browser, "you are about to see a file written in HTML version 5"
* although this line of code has changed over the years, this is the most recent version

`<html lang="en"></html>`
* we can see in the code these two actually is separated, and therefore also encapsulats some other content
* the first pair of angle brackets `<>` is so-called *open tag* or *start tag*; the corresponding pair therefore are so-called the *close tag* or *end tag*
	* the pair are the same, except only the open one would have attribute and the close one would have a slash `/`
* `lang="en"` means the language of my page, is written in English language
	* humans have standardized 2 letters, sometimes 3 letters, codes for every human language
		* `"en"` for English
	* this is just a clue to the browser for like automatic translation and accessibility purposes, what language the web page itself is written in
		* not the language of the tags
		* but the words, like `hello, title` and `hello, body`
	* also notice this is yet another example of key value pairs
		* key is an attribute which is `lang`; value is `"en"`.
		* probably the browser is using a [[CS50x 5-4 Hash Table|hash table]] underneath the hood, to keep track of this
* finally between the pair is many lines of code. notice we would indent everything inside the pair.
	* it's not important to the computer
	* just easier for us human to read, since it implies some hierarchy
		* indeed what are inside the the html tag is the head tag and the body tag
		* the head and body tag are two children of the html element
			* which is to say head and body are now siblings

`<head>` tag
* the head of the page
* it's just the address bar and other such stuff at top
* in this case it has only 1 child, which is the `title` element
	* nowadays titles are typically embedded in the browser's tab
	* the title element has 1 child, which is just a pure text, otherwise known as a *text node*

`<body>` tag
* the body of the page
* the body is like 99% of the user's experience, since it's the big rectangle window
* this time it too just has 1 child which is a text node, `hello, body`

although the browser doesn't care about the indentation
what's so nice about it is that it implies a structure like this
![[html-structure.png|500]]

what does it look like?
a [[CS50x 5-3 Tree|tree]] structure
* a special node refers to the document type
* the root node `<html>`, under it is 2 children
	* `<head>`
		* contains a child `<title>`
			* contains a child text node
	* `<body>`
		* contains a child text node

also see why texts in html are called text nodes now?
since everything in a html is like a node in a binary tree structure

> [!Recap]
> 1. When our browser, Chrome, Safari, whatever, downloads a web page, opens up that envelope and see the contents that have come back from the server.
> 2. It reads the HTML code, that someone wrote, top to bottom, left to right
> 3. And create in the browser's memory, in our device's memory, or RAM,
>     a data structure like the above image.

And actually this is already what HTML is all about.
* Above are the fundamentals, we would see some more tags and attributes down the road
	* but they are the kind of thing that we look them up when we need to
	* and eventually many of them get ingrained
* To assemble the structure of a web page, it's nothing more complicated than the above `hello.html` example
	* just tags and attributes
___

# HTML Validation

Have you ever wondered what would happen if we put `<title>` tag under `<body>`?
The browser simply don't show it up
* Underneath the hood indeed is an error
* But browsers tend to be pretty generous, so they try their best to recover from our mistakes
	* sometimes the text might display, or might not, or not display as we intended
	* also it might not display the same on Mac's Chrome or PC's Chrome or Firefox or Safari

There are tools though that help us find out the mistakes
eg. https://validator.w3.org/
* w3 is the World Wide Web Consortium
	* a group of people that standardize things related to the Internet
* in that website we can place our html structure either by the format of URI, html file, or text
* then it can check the structure for us to see if any mistake or error
___

# Well-formed HTML

* Every tag opened needs to be closed.
	* Tags we opened in a particular order should be closed in reverse order.

* Unlike C, syntax errors won't necessarily make our HTML page fail
	* the HTML may be not well formed but would still work
	* errors could slide by
		* sometimes it makes our page fails
		* sometimes it doesn't
* but off course no error at all would be the best

* Remember to use [[#HTML Validation|HTML validators]] to check if the HTML is well-formed!
___
