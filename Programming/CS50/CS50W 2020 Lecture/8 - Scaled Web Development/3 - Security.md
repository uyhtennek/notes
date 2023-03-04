now we turn our attention to security
and actually, all of the topics that we covered in this course, have potential security issues
___

# Git and Open-Source

increasingly, a lot of softwares are becoming open-source
* anyone can see and contribute to the source code of an application

it's great for collaboration,
but it also means that we have to be very careful when it comes to credentials or sensitive stuffs
* because important information can leak inside of the source code
* eg. generally, never put passwords or things like that, inside of a Git repository
	* because any one can look at that

for example, you have a commit where some credentials are exposed, then you removed that and commit and again, so that the credentials are not there any more
* but actually, people have access to not just the latest version of your code, but every version of your code
* so they can roll back and see your credentials in your previous commit
___

# Phishing Attack

really it just comes down do something like this
```html
<a href="url1">url2</a>
```
* an anchor tag that directs users to `url1`, but it looks like it's directing users to `url2`

eg.
```html
<a href="https://cs50.harvard.edu/extension/web">https://www.google.com</a>
```

this is a very common attack vector
* typically happens in emails
	* you might see an email that tells you to click on a link but that link takes you to somewhere else entirely instead
* as a result someone might inadvertently share their bank account credentials or the likes

so, be mindful where links actually taking you
* in most web browsers, if you hover over a link, it will show you where that link goes to
	* because it might appear different to the text of its anchor tag
___

# Fake Website

since HTML basically contains every information about a web page, we can fake a web page easily
* eg. go to https://bankofamerica.com, View Page Source, then copy all the content and paste that in our own HTML file
* then we can open up our page, and it appears the same as Bank of America's page

then, we can fake people to enter our fake Bank of America page
* maybe using a phishing attack
* then maybe people would believe it, and inputting their bank credentials to try to log in Bank of America's website, so the private information is leaked

therefore, be mindful that, any one can copy the HTML you wrote
* so anyone can theoretically pretend to be you
___

# The Internet

recall that any one computer, communicate with another computer, using HTTP and HTTPS
* recall that HTTPS is a more secure version of the HTTP protocol
	* and recall that they are protocols that about how information gets from one person to another, and what we're storing with that information
* and to do so, information are going to flow through the Internet, which is a bunch of routers
	* there are many intermediate routers

as a result, how can we know that if our information that is getting passed back and forth, is getting passed back and forth, securely?
* because when we're sending emails, we don't want any of those routers to be able to look at that request and see the contents of our emails
	* likewise, it's the same for passwords
* therefore, we'd like these information to be encrypted

so how encryption works?
___

## Secret-Key Cryptography
we use a secret key to encrypt and decrypt information

1. using a *key*, we encrypt the *plaintext* into *ciphertext*
2. we send the ciphertext, across the Internet, to the other person
3. using the same key, the other person can decrypt the ciphertext into plaintext

this key is what we might call, a *symmetric key*
* we use the same key in order to both encrypt and decrypt
* as long as we and the other person have the key, we'll be able to encrypt and decrypt messages
* all the middle men just have the ciphertext, but not the key, so they won't be able to figure our what that original message was

but there is a problem, that is both we and the other person need to have access to the same key
however, we can't just send the key across the Internet to the other person
* because that leaks the key, then people in the middle can decrypt our ciphertext

now, if we were able to go to the other person in person, and exchange the secret key, in secret
well then, the problem is gone
* however, it's not very practical
* because generally when communicating on the Internet, we're not communicating to servers that we communicated with before
* if we try to make a request to a new website, we face the problem again
___

## Public-Key Cryptography
this is a major advancement in cryptography, that allows for the Interent to work

in secret-key cryptography, the key must be kept secret in order for the scheme to work
in public-key cryptography, the key is allowed to be public
* or one of the keys is allowed to be public

the idea here is that, we're using two keys, instead of just one
* we have a *public key* and a *private key*
* the private key, as its name proposed, can't be shared, to keep the encryption scheme secure
* but the public key, is okay to share

* the public key is used to encrypt information
* the private key is used to decrypt information, that is encrypted by the public key
* and the public key and the private key, are mathematically related

1. I want to communicate with another person
2. that person sends me his public key
* it's okay that the public key travels across the Internet, anyone is allowed to see it
	* because it's only used for encrypting information
3. I can then use the public key to encrypt my plaintext, to generate a ciphertext
4. then I send the ciphertext to the other person
5. using the private key, the other person can decrypt the ciphertext, to read my plaintext

and now, two computers that have never interacted before, without having the opportunity to meet to exchange secret information, can use this technique to securely communicate

and then once they are interacted, they can agree on some secret key, and then they can start to use secret-key encryption and decryption, then they now can have less things to send across the Interent

ultimately, this scheme allows HTTPS and the Internet to work
___

# Storing Passwords

we have applications that have users
* they can sign in with a username and a password
* but how might that information be represented?

one way, is simply, store them as plaintext
| id | username | password |
| :-: | :-: | :-: |
| 1 | harry | hello |
| 2 | ron | password |
| 3 | hermione | 12345 |
| 4 | ginny | abcdef |
| 5 | luna | qwerty |

it turns out, this is incredibly insecure
* we should never do this in practice
* if someone unauthorized somehow get access to this database, they would be able to see all of the passwords of all of the users
	* this kind of thing does happen, database does get leaked

the recommended approach, is to save the hashed version of the same password
| id | username | password |
| :-: | :-: | :-: |
| 1 | harry | 48c8e8c3f9e80b68ac67304c7c510e9fcb |
| 2 | ron | 6024aba15e3f9be95e3c9e6d3bf261d78e |
| 3 | hermione | 90112701066c0a536f2f6b2761e5edb09e |
| .... | | |

a *hash function* is some function that takes something as input, in this case a password, and outputs some *hash*
* which is some sequence of characters and numbers, that represents the password
* and the important thing is, it's a one-way hash function
	* we can use a password to get this sequence of characters and numbers
	* but it's very very difficult to go the other way around

therefore, the company won't actually know what any particular user's password is
* when a user try to log in, they just take the password, and hash it, and compare that hash against the hash stored in the database
	* if the hashes match up, that means the user typed in the password correctly, so we can believe it's the same person, and we sign the user in
	* otherwise, that's a sign that the user didn't type the password correctly
* in addition, this is the reason why companies usually can't tell you what your password actually is, if you forget your password
	* they don't know your password, so they can just let you reset your password
		* they only know some hashed version of the password, and whether or not you entered the correct credentials
	* because they can only update the data inside of the table
___

# Reset Password

if you ever forgot password, you might get to a reset password page
* you type in your email address and hit a button
* it sends you the email for you to reset password

if you typed in your email, it informs you that "Password reset email sent."
if you typed in some random email, it informs you something like "Error: There is no user with that email address."

now this is leaking information, about which users happened to have accounts
* although this might not be a big deal, you might decide this is not something worth caring about securing
* if you do care, you should keep the information of if someone has an account or doesn't have ana ccount, private and secure only to the user
* and this type of user interface is leaking that kind of information
___

# Response Time

we can even leak information, just based on the time it takes for the database to be able to respond to a particular request
* eg. if you make a request and you notice it's taking longer than usual to respond, that might tell you something about the number of database queries it needs to run or the amount of information that's stored about the user
* so even something like how many milliseconds it takes for a web server to respond, can reveal or leak information

eg. there have been researchers who actually tried and see what information that they can get, from looking at this kind of information
* the kind of information that doesn't seem like it would leak information, but might actually reveal information
___

# SQL Injection
details in [[2 - SQL concepts#SQL Injection|previous lecture]]

```sqlite
SELECT * FROM users
WHERE username = username AND password = password;
```

```sqlite
SELECT * FROM users
WHERE username = "harry" AND password = "12345";
```

```sqlite
SELECT * FROM users
WHERE username = "hacker"--" AND password = "";
```

to deal with this, we want to escape any potentially dangerous characters, that might show up inside of our SQL queries
* actually Django's models do this for us
	* it's taking care of the process of making sure that we're safe from these kinds of SQL injection attacks
* but if ever you're writing a web application that is directly executing SQL code, do make sure your program is safe from these kinds of threats
___

# APIs
we want APIs to be scalable and secure as well

Rate Limiting
* we might want to make sure that no user is able to make more than a certain number of requests to an API, in any particular amount of time
* so that the server is safe from DOS Attack, or *Denial of Service Attack*
	* meaning someone just make a whole bunch of requests to a server over and over again
	* to a point that the server is no longer able to handle that many requests all at the same time
* you can imagine that we can produce a DOS attack via a few lines of code
* so APIs will often some kind of rate limiting
	* so that people can't overwhelm the server or the database

Route Authentication
* we might not want everybody to access the same data via an API
* maybe some APIs should only be accessible for the admins
* we can do this by giving the user an API key
	* effectively it's like a password
	* they need to pass that key anytime they are making an API request
	* then we can look at that key and verify that they are authenticated
* again, generally we don't want to put these API keys inside of our source code
	* much like [[#Git and Open-Source|why we shouldn't put passwords in our code]]
* one common solution to this, is using *environment variables* to generate API key
	* so our API key is not going to be some predetermined string
	* but it's drawn from the environment in which the program is being run
	* then on the server, we need to make sure that we have those environment variables set correctly, therefore it knows what the key should be without actually having the key to be inside of the web application source code

you might notice that many APIs will require us to have an API key
* often, for these sorts of reasons
___

# Cross-Site Scripting

In general, when on our web application, we only want JavaScript to run, if we have written it.
* because someone else might be able to get JavaScript code, that they wrote, to run on our web
* then if someone else can run their own JavaScript, they can manipulate the contents of our web
	* and the result of that might not be actually desired

eg. let's say we have a web app that allows any path, and then the user tried this route
* `127.0.0.1:8000/<script>alert('hi');</script>`
* and if someone sends this link to others, when they clicked the link, they get to our website, but at the same time they are tricked to run some JavaScript that are not created by us
* and the JavaScript can do all kind of stuffs
	* eg. changing the content of the page, making API requests, etc.

therefore in our web app, we should detect if this kind of injection is happening, or escape it in some way, or take other precautions
* to make sure this kind of cross-site scripting isn't going to be possible

eg. in a messaging app, maybe sometimes we need to send some JavaScript code to others, and we don't that JavaScript code ends up running all the sudden
* we want to escape the information, so that it's text but not code

it's kinda like [[#SQL Injection]]
* the idea is that we don't want others to be able to inject their own code into our program
___

# Cross-Site Request Forgery
you might recall that [[2 - Web App Examples#POST request|Django is quite good at defending this one]]

we'll explore more how it works in details here
* a cross-site request forgery is that, we fake a request to a website, when we didn't intend to actually make that request
* eg. if we're making a bank app, if transferring money is just a link with some get parameters
	* then people can do something like this
```html
<a href="http://yourbank.com/transfer?to=brian&amt=2800">Click Here!</a>
```
* and then if somebody else clicked on that, they just make a request to our bank to transfer $2,800 to Brian
	* this website just forged a request to our bank
	* it fakes us to believe that that user want to transfer money to Brian

in addition, it doesn't have to be a link
```html
<img src="http://yourbank.com/transfer?to=brian&amt=2800">
```
* although it looks strange, because the link isn't an image, it doesn't matter
* because how image tag works, is by making a request to the source URL
	* and then when they get back that image, they display it in the user's web browser
* and note that, the user doesn't click on or do anything here
	* image tags can initiate an unintended request without even the user even realizing it

therefore, we suggest that anytime you're creating a website that is going to allow for the manipulation of some kind of state, that allows for some change to happen
* something like transferring money
* don't make it a GET request
	* because it's easy to fake
* we should use POST requests, when it comes to manipulating things inside of the database
	* but even then, a POST request isn't secure because it's still can be fake
```html
<form action="https://yourbank.com/transfer" method="post">
	<input type="hidden" name="to" value="brian">
	<input type="hidden" name="amt" value="2800">
	<input type="submit" value="Click Here!">
</form>
```
* it just takes more typing to fake, but it can be fake

you might think it's not a big deal, because it still take clicking on the button
* we can totally avoid this simply by, not click on the button
	* which seems reasonable because why would we want to click on a button that we don't know what it's going to do?
* but one, an adversary can make the button looks safe to click or trick the user somehow
* and two, an adversary can do this
```html
<body onload="document.forms[0].submit()">
	<form action="https://yourbank.com/transfer" method="post">
		<input type="hidden" name="to" value="brian">
		<input type="hidden" name="amt" value="2800">
		<input type="submit" value="Click Here!">
	</form>
</body>
```
* so an adversary can push one step further, to do auto-submitting

So how do we guard against this?
* a very common approach, also the Django's approach, is to add a CSRF token
	* that is going to be regenerated for every session
	* so that, only if that token is present, the form is valid, and only then the transfer will be able to go through
```html
<form action="/transfer" method="post">
	{% csrf_token %}
	<input name="to" value="brian">
	<input name="amt" value="2800">
	<input type="submit" value="Transfer">
</form>
```

if other websites try to forge a request, their forms can't go through
* they don't know what the CSRF token should be, because it changes for every session
___

> [!info] Security in Web Applications
> As we're developing our web applications, in addition to think about what to add to and what features to support in our web applications, to think about what the potential vulnerabilities there are as well.
> 
> How someone might exploit our web application in order to do something bad for our users.

