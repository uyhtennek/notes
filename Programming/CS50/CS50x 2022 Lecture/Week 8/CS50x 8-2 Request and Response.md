# DNS
*domain name system*

So in fact, all the information on the Internet, are sent from and to some IP addresses.
But wait, when we send a email we didn't care about IP addresses right? Heck we probably don't even know our own IP address or others IP addresses.

* These days it's like we don't know most of our friends' phone numbers.
* Indeed when we visit a website, what do we type in?
	* typically not `#.#.#.#`
	* instead we type in a domain name
	* eg. `Stanford.edu`, `Harvard.edu`, `Yale.edu`, `gmail.com`, etc.
	* eg. `google.com`, `cs50.harvard.edu`
* But still, `google.com`, `cs50.harvard.edu`, etc. those are, in fact, IP addresses
* After all, how could we remember a bunch of `140.247.223.81` or `2001:4860:4860::8844` or the likes, when we can't even remember our friends' phone numbers....
	* a bunch of words would be much easier and much more comprehensible

Every network, no matter it's Harvard's, Stanford's, Yale's, our home's,
somehow has a DNS server
* you probably don't know about it because someone else did configure that for us
	* either it's our campus, our job, or internet service provider
* which our devices connect to

Then there is this big table, a big spreadsheet, or technically a [[CS50x 5-4 Hash Table|hash table]], that has
* domain name as the key
	* eg. `Harvard.edu`, `Yale.edu`
* IP address as the value

That is to say, a DNS server's purpose in life
is just to translate domain names to IP addresses. And vice versa.
* technically, just to be precise, it translate *fully qualified domain names* to IP addresses

again, such things are happening magically when we turn on our phone or laptop today
because these things are pre-configured for us

can think of DNS is like the yellow pages of the web,
in this context it's just a huge list running from `0.0.0.0` to `255.255.255.255`
| Host | IPv4 Address | Host | IPv4 Address |
| :-: | :-: | :-: | :-: |
| `info.host1.net` | `0.0.0.0` | `io-in-f138.1e100.net` | `74.125.202.138` |
| `info.host2.net` | `0.0.0.1` | | ... |
| | ... | `info.hostn.net` | `255.255.255.255` |

btw `io-in-f138.1e100.net` actually is a real host name
* IP address `74.125.202.138` actually is `google.com`
* But Google has a lot of different servers, and they can't all be called `google.com`
* So Google has its own internal system for translating `google.com` to one of its servers IP address

`lk-in-x93.1e100.net` is another one
* IP address is `2a00:1450:4010:c09:0:0:0:93`
* it's `google.ie`, but an Irish one

one more thing, so DNS is like the yellow pages, but that would be storing 4 billions IP addresses for IPv4 and then at the same time storing some crazy numbers of IPv6 addresses
* thinking of a real world yellow pages, if you know what it is..., we don't get a book that contains every phone number that exists on the planet right?
* We only get the local phone numbers
* DNS is just the same
	* the DNS server in our home just contains the IP addresses of our home network
	* large DNS servers like `google.com`, are like libraries, that have a copy of every DNS
		* collecting some small scale, local DNS information
		* and updating them frequently, which just means collect the information one more time
			* so the small local DNS servers do need to update their information themselves
* DNS record sets are thus fairly decentralized
___

# Access Points

That's the network that we choose when we connect our device to the Internet
Here we just touch on this topic in general but not in details

Recall [[CS50x 8-1 The Internet#TCP/IP|IPv4]] can only give roughly 4 billions IP addresses right?
Then we would be already run out of long time ago
The thing is, we didn't give all the devices in the world, each an unique IP address
* we instead assign an IP address to a router
* everyone connecting to this router, or access point, would use the same IP address to get out
	* so everybody at our home has a private IP address
	* we didn't actually connect to the Internet directly, the Internet can't speak directly to our computers too
* instead we're connected to the outside world through the router, and it's the router's job to
	* take the information that we're sending to the outside world, to the correct place
	* take the information that is sent to us from the ouside world, and send it to us

The router at our home is the device that has the public IP address
* that's the device and perhaps typically the only device in the room that connected to the Internet
* and we, our devices, connect to the router

Typically a modern home networks would have a access point that combine a router, a modem, a switch, and other technologies togeter into a single device
On the other hand modern business networks, or large-scale *wide-area networks*, still frequently separate them, to make it easier to control things
* WAN is the shorthand for wide-area network
* a network can has multiple access points
	* WANs probably gonna need multiple access points as just single one is not enough to cover its large area
	* eg. at a university there are things look like routers mounted all around campus
___

# HTTP
*hypertext transfer protocol*

* the `http` part in URLs, like `https://www.google.com`
	* usually our browsers would add that for us automatically and we just need to type in `Harvard.edu` or `Yale.edu` or the like
* it standardizes how web browser and web server inter-communicate

This is a distinction now between the Internet and the web
* The Internet is really like the low-level plumbing
	* the cables, the technology, that just moves packets, or gets data, from one place to another
	* we can do anything on top of that Internet
		* sending email, browse web, watch video, chat, gaming, etc.
* The web, or HTTP, is just one application, built on top of the Internet
	* meaning that [[CS50x 8-1 The Internet#TCP/IP|TCP/IP]] has nothing to do with HTTP
* Kinda like electricity and all the interesting products that use electricity
	* and we all probably don't even know or care how they works
	* but now we'll be programming for the web, so unfortunately we need to understand the Internet and how these kind of things indeed work

Firstly, to wipe out some possible confusion from the get go,
these days, it usually HTTPS instead of just HTTP
* S stands for *secure*
* HTTP is the main thing
* they are similar things

When a data get to its destination port, or program,
our computer actually doesn't know what type of data it is,
although usually the application or the program would has its own system of rules to interpret the incoming data, or the received data

Well, HTTP is a set of rules that just says data under my rules must use some certain formats
HTTP standardizes what kinds of messages, or data, go inside of these envelopes, TCP/IP packets
* typically it's just textual information, a simple text format
	* some humans decided on the formats years ago
	* as its name says, it's a set of rules about transferring hypertext
		* in other words it's a set of rules on how to format the text when it comes to web requests and responses
* which tells a browser how to request information from a server
	* how one must make a request for a web page
* and how servers respond to the clients with information
	* how servers that host web pages, deliver the information back to clients
* it's an example of *application layer protocol*

> [!info]
> Other application layer protocols include:
> * File Transfer Protocol (FTP)
> * Simple Mail Transfer Protocol (SMTP)
> * Data Distribution Service (DDS)
> * Remote Desktop Protocol (RDP)
> * Extensible Message and Presence Protocol (XMPP)
> 
> Application layer protocols are used,
> Every time we're using a service, which is expecting to receive a request, in a very particular format and then is required to return information back, as well in a very particular format.

If Google is a person, if we want to visit `google.com`, we would just ask him "Hey can I visit your home page?"
Since Google isn't a person, it's a server containing the home page, we need to follow the HTTP format to ask for its home page, which looks like
```
GET / HTTP/1.1
Host: google.com
...
```
see more in [[#Request and Response|below]]
___

# URL

For instance, here is a canonical URL
`https://www.example.com`


1. path
* what's hidden in our sight, is a slash `/` at the end
	* the full URL should be `https://www.example.com/`
	* browsers nowadays tend to simplify things, and don't know that to us
* the slash `/` just represents like the default folder, the root of the web server's hard drive
	* like `C:/` on Windows, or the Macintosh HD on mac OS


but an URL can have more than that, it can have `/<path>`, where path is just a word or some words
`https://www.example.com/path`
* that `path` can actually be a specific file
	* `https://www.example.com/file.html`
* or it can be a folder
	* `https://www.example.com/folder/`
	* or maybe a file within a folder
	* `https://www.example.com/folder/file.html`

* browsers nowadays like Safari, Chrome, etc., tend to hide things and details
* ultimately though we do need to understand what URLs that we're at, because it maps directly to the code that we're going to write


2. fully qualified domain name
* before, we talked about [[#DNS|domain name]]
	* it's the `example.com` in `https://www.example.com/<path>`
* so where is the fully qualified domain name?
	* it's the `www.example.com` part

What is `www`?
* it is known as *hostname* in the context of web
	* or alternatively *sub-domain name* in the context of email address
* it is referring to a name of a specific server in that domain
* back in the day, there was a server named `www.example.com`
	* if it was a mail server maybe its name is `mail.example.com`
	* for a chat server maybe it's `chat.example.com`
* nowadays, it can actually refer to a whole bunch of servers
	* if we go to `www.facebook.com`, that is not just one server, right?
	* that's thousands of servers nowadays
* So long story short, there is technology related to this part that somehow will get us to one of thousands of servers


3. Top level domain
This time it's the `com` part

There are many of them
* `.com` means *commercial*
	* although anyone can buy it these days, even for non-commercial use
* `.org`, `.net` are something similar
* some of them are a bit restricted
	* like `.mil` is just for the US military
	* `.edu` is just for accredited educational institutions
* `.io` means it's something related to input-output
	* like `CS50.io`
	* although, fun fact, `.io` actually belongs to a small island nation, whose country code is in fact `.io`
* also there are other country-specific top level domain
	* `.uk`, `.jp`, and the likes
* there is `.tv` as well, where `tv` stands for television


4. [[#HTTP|protocol]]
lastly, this part is the `https`
that specifies how the server uses this URL to get data from point A to point B
___

# Request and Response

Okay now we're done talking about the envelope itself.
Next up is the things inside of the envelope.

It essentially is one of 2 verbs
`GET` or `POST`
* They describe just how to send information from us to the server
	* `GET` means put any user input in the URL
		* direct HTTP request
	* `POST` means hide it
		* like the things we're searching for, credit card number we're typing in, usernames and passwords,  it makes these stuffs don't show up in the URL
		* so it makes them invisible to anyone with access to our computers and search history
		* but still they're somehow provided elsewhere, deeper into that envelope
* There could be others but we would skip them for now
* In fact we'll mainly just focus on the `GET` here

Let say we opened our laptop, then we entered a URL, like `harvard.edu`
so we are start connecting to the Harvard's server, and requesting a web page
* it would be a two-step process
	1. the laptop **request** something from Harvard's server
	2. the Harvard's server **response** something to the laptop
* typically, the laptop would be called **client**, the Harvard's server, sure enough, is called **server**
* like in a restaurant, client would request something to eat, and the server would bring it to us

Technically, the request, the thing inside that envelope, would look something like this
```
GET / HTTP/1.1
Host: www.example.com
...
```

so it indeed is a textual message
that our computers or devices are automatically generating

the first line is called an HTTP request line
it's format is `method request-target http-version`

Here is what is going on with the text
* the slash `/` up there is the URL's path
	* represents the default page on the website
	* the hidden slash `/` of the URL
* `HTTP/1.1` is the version of HTTP
	* now we're up to version 2 and version 3
		* but `1.1` is quite common
		* there is also `1.0` which we don't use any more
	* meaning this conversation is under this protocol
	* kinda like it's the lanuage of this textual message
* also there is the `Host: <fully qualified domain name>` or `Host: www.example.com`
	* since a single server can actually host many different websites
		* think about some popular hosting websites like Squarespace or Wix
		* they wouldn't offer to us our own personal server when we ask them to host our websites right?
		* many other customers' websites are hosted by the same server as ours
	* how a server, that is hosting multiple websites, would know which website that the client is talking about, or requesting from?
	* That's why the fully qualified domain name doing here
	* together with the `request-target` they specify a specific resource in a specific server, or host, being sought
* `...` meanings there are some more things going on with it, but the essence is really just those few things
	* these 2 lines are just the very beginning of the message
	* kinda like the Dear Example part, if it was a letter

* it's like when the envelope is delivered to the Squarespace's or Wix's server, at this point the servers don't know what the envelope is doing here right?
* only when the servers open up the envelope, and see the content inside, then it knows
	1. oh, the client requested the `/` page from me
	2. that is from the one of the website that I'm hosting `www.example.com`
	3. and I need to handle it with the `HTTP/1.1` protocol
	4. also there are some more stuffs going on but we'd just abstract those away

Okay that's done for the request part
Next thing, the server reads our request, then it will give us back a response
```
HTTP/1.1 200 OK
Content-Type: text/html
...
```

* `HTTP/1.1`, so the response packet is still handled with the same protocol as the request packet
* `200 OK`
	* `200` is a status code
	* and `OK` is exactly just mean OK, that the request was successful and satisfied
* and that also indicate the type of content that is coming back
	* which, too, is standardized
	* `text/html` just means here comes some text in HTML format / language
	* it could be `image/jpeg` or `image/png`, or `video/mp4`, etc.
	* these are known as *MIME types*
		* they uniquely identify types of files
		* similar in spirit to file extensions, but for packets
			* also therefore it's a little more standardized
* also there are some more stuffs in `...`
* at the very bottom of the response, is actually the HTML
	* the content of `example.com`'s home page

one last thing these textual messages. these are called **HTTP headers**.
and we actually would see again the key-vair pattern
* like `Host: www.example.com` and `Content-Type: text/html`
* and the things going on in `...` would be something following the pattern `<key>: <value>`
___

# Visiting `harvard.edu`
Why and how the browser would sort of auto-correct the URL that we typed in?

There is a technique to diagnostic things happening
that is to open the [[Cybersecurity#^8fef5b|Incognito mode]]
* not because we care about our browse history
* but because it throws away any history
	* so every request is going to look like a brand new one
	* we always going to see fresh information

There is also Developer Tool, which any common browsers come with
* now if we open up the Network tab, and then visit `http://harvard.edu`
* we would see there are hundreds of requests and perhaps responses too going on
	* downloading 27.5 MB of data
![[visiting-harvard.edu-requests.png]]

Why is that? I mean, we just sent 1 request initially by entering `http://harvard.edu`.
* When the server receives that initial one request, then it goes
* "Okay by the way there are 112 other things that you also need to get"
* so our computer does also request these extra content

We can click on each request to see some diagnostic information.
* which probably no body need to care about
* but here we do can see the HTTP headers, the **Request Headers** and the **Response Headers**
	* however we need to click `View source` to see the source header
	* to see the source textual message
![[visiting-harvard.edu-headers.png]]

Now let's just scroll to the top one and focus on the very first request that we sent
and looking at its Response Headers
![[visiting-harvard.edu-301.png]]

* Interesting enough, it's actually not OK. It's not `200 OK`
* Instead it's `301 Moved Permanently`
	* meaning `harvard.edu` is moved permanently
	* but where to?
* and we can see there is a line saying `Location: https://www.harvard.edu/`
	* this is an HTTP header, a standardized key value pair
	* it's part of the HTTP protocol
* the response headers in short just means "Harvard's web page is not at `harvard.edu`, let me redirect / bring you to where it's belong which is the location `https://www.harvard.edu/`"
	* why particularly `https://www.harvard.edu/` though?
	* maybe someone in Harvard want us to use a secure connection
		* so it leads us to `https`
	* maybe the marketing people want us to be at `www` instead of just `harvard.edu`
		* but also there is technical reason we prefer to use a hostname but not just the raw domain name

* next when the browser see a `301` response like this, it knows, by design, by the definition of HTTP, to automatically redirect the user
	* so actually users don't need to know or care about all of these domain name, hostname, HTTP things
	* but still, can get to the page that they ask for
* the browser was told to go elsewhere, the new location, and then it just followed those breadcrumbs, eventually it downloaded all of the images and files and so forth, and in the end compose the web page

also try `http://safetyschool.org`
it's been like a joke for like 10 or 20 years, someone out there has been paying for the domain name for 10 or 20 years...
and if we go look at its response headers, we can even see its sole purpose is just to redirect visitors...
___

# Curl
a tool from VS Code, runs on a terminal
* for connecting to a URL
* sort of like a mini-browser
	* allowing us to play with websites and just see those headers
	* without bothering to download all the images and text and so forth

To browse a website's headers, enter
```bash
curl -I -X GET http://harvard.edu/

# then it'll just give us the headers
HTTP/1.1 301 Moved Permanently
Retry-After: 0
Content-Length: 0
Server: Pantheon
Location: https://www.harvard.edu/
...
```

And we can do the next step manually
```bash
curl -I -X GET https://www.harvard.edu/

# output
HTTP/2 200
...
```
___

# Status Codes

But what if we go to a invalid URL?
```bash
curl -I -X GET https://www.harvard.edu/thisfiledoesntexist/

# output
HTTP/2 404 Not Found
Content-Type: text/html
...
```
format of the first line, the HTTP response line, is `http-version status`

Perhaps we all are quite too familiar with the error code 404
* when we have typo in the URL
* when someone deleted the web page

Usually even the web page is not found,
the server will still lead us to another web page, some 404 page

It's a little clue sent from the server to our browser, about what the actual problem is.
Based on whether the resource exists and whether the server is empowered to deliver that resource given the client's request, a number of status codes can result
```bash
# Success
200 OK  # All is well, valid request and response

# Redirection
# Related to redirecting user from one place to another --
301 Moved Permanently  # Page is now at a new location
	# most browsers would automatically redirect us to there
	# unless it's a really out-of-date browser
302 Found  # Page is now at a new location temporarily
304 Not Modified
307 Temporary Redirect

# Client Error
# Typically happen when we mess up our password
# or try visiting a URL that we're not suppose to look at
401 Unauthorized  # Page typically requires login credentials
	# like just jumping right into a user's profile in Facebook without being logged in
403 Forbidden     # This request just isn't allowed no matter what
	# the resource exist, but you are not allowed to access it
404 Not Found     # Server cannot find what was asked for

# It's an April Fool's joke  :-)
# by the tech community years ago
418 I\'m a Teapot

# Server Error
# Real bad stuffs
# Kinda like the Seg Faults in the world of C, but don't worry it too is solvable
500 Internal Server Error  # Generic server failure in responding to a valid request
503 Service Unavailable    # maybe the server is overloaded
504 Gateway Timeout        # server somehow can't deliver the resource before timeout
...
```

fun fact, Facebook used to be on, not `www.facebook.com`, but `www.thefacebook.com`
so if we make a request for `www.thefacebook.com`, we would get the `301` response
___
