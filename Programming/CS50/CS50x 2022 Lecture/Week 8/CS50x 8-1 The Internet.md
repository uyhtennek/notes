# Internet

What is Internet?
* Big storage. A big cloud.
* A bunch of data that we all can reach.
* It's indeed a hardware thing. It's whole lot of servers out there, that are somehow interconnected
	* via physical cables
	* via Internet service providers
	* via wireless connectivity
	* and the like
* It's a bunch of network, and network, and network, somehow interconnected
	* Harvard has its own network
	* Yale has its own network
	* Our home has its own network
* As we connect these networks, we get the interconnected network, the Internet

And networks can talk to each other
* maybe Google's network has something that I'm looking for
* maybe I just ask them for some cat photos, then it would give these to me

ARPANET
* From the *Advanced Research Projects Agency*
* one of the firstmost Internet
* purpose is to inter-connect a few universities in Utah and California
	* servers / computers in these areas somehow interconnected with wire
	* so people can start to share data

![[ARPANET-1969.png|600]]

* A year later it expanded to include MIT and Harvard and others
![[ARPANET-1970.png|600]]

* Fast-forward to today, the network becomes something like this
![[Internet-2022.png|600]]
___

# How Internet works?

What are the nodes? Are they just server or computer?
* Well, yes they are. But they are a certain type of server, namely *router*.
* As its name implies, it routes the data.
	* A piece of data is sent into our home, does it go this way, that way, any other way? To your computer, to your dad's computer, to your phone or your sister's phone?
	* That's the router's job.
	* But certainly for some areas as big as Harvard's or Yale's or Comcast's campus, there are probably some numbers of interconnections instead of just 1 like the home example.

Next, how to get data among these routers?
* eg. send an email to someone at Standford, in California from the East Coast
* eg. when we visit www.standford.edu, how does our devices get data from their servers

1. When our phone or laptop boots up
* it already know what the local router is, or what the address of the local router is

2. if we send an email
* the email is handed to the nearest Harvard router

3. the router yet again hands the email to another router, one step toward the target router, from Stanford maybe
* some number of steps and routers after, the email gets to maybe a Boston's router, and then maybe a California's router
* and eventually get to Stanford's router, and finally the email reaches Stanford's email server

But how do each router knows which is the next router to hand the packet over to?
Long story short, there is algorithm that help the router decide just that.
* Perhaps the algorithm takes in an input, that tells something about the information, something like where is the destination of this email
* What is also happening is a thing called TCP/IP.
___

# TCP/IP
refers to 2 protocols
* conventions that indicate how computers communicate
* a set of rule that computers behave
	* that any network need to follow, for the things to work

Like in the human world
* someone extends his hand to us, we shake his hand
* in these day we have mask protocols - we need to wear masks outside

Computers too use protocols all the time
* how they send information
* how they receive information

So what is TCP?
* it's like a virtual envelope
* if we send a email, the email itself is in a virtual envelope, so to speak
* outside of this virtual envelope, are some address
	* the address that we send our email to, the destination
	* our address
* also, if needed, we can actually send the email back from destination to source address
	* by just flipping the 2 addresses

Then what's IP?
* IP stands for [[#Internet Protocol]]
* IP address is not as simple as email address
* Computers on the Internet have unique addresses of their own
	* it's the IP address
	* a numeric identifier
	* typically its format is `#.#.#.#`
		* each `#` is a number from 0 to 255
		* eg. `123.45.67.89`, this is actually a generic default IP address
		* eg. `140.247.223.81`, IP addresses from Harvard University starts with `140.247`
			* it also tells where geographically the device is
		* [[CS50x 0-1 Information in computer explained#Byte|recall]] 256 number is how many byte? 1 byte or 8 bits.
	* therefore an IP address must use 32 bits, or 4 bytes.
* btw, ever wonder why not make it hexadecimal, like [[CS50x 4-1 Memory#Why hexadecimal?|memory addresses]]?
	* maybe like `ff.ff.ff.ff`? it's one less digit, kinda make sense?
	* in the end we choose to make it more friendly to the people who don't know hexadeciaml

* It looks like we have 4 billion some unique IP addresses available to us
* but there are a hugh number of humans these days, all of which have multiple devices
	* laptop, phone, IOT things, etc.
* 4 billion is not quite enough
* btw, there are some workarounds, although they are not how we address the problem ultimately
	* private IP address
		* basically it fakes a unique IP address
		* by funneling the IP address through one single address, which is shared by many different computers
		* won't last forever though

`#.#.#.#`
this is version 4 of IP, IPv4
there is also a version 6 of IP, IPv6
* that instead of using 32 bits, it uses 128 bits
* and it gives a crazy number of possible addresses
* ~~4,294,967,296~~ --> 340,282,366,920,938,463,463,374,607,431,768,211,456

Although IPv6 has been around for a while,
only in the last couple years we have actually started phasing in IPv6 to phase out IPv4

> [!info]
> Often times in the world of computers and computing, we're good at anticipating problems but really bad at dealing with them even though we know about them.

IPv6
* `s:t:u:v:w:x:y:z`
* each letter represents 1 to 4 hexadecimal digits
	* `ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff`
* we jump from 8 clusters of 4 bytes, from IPv4, to 8 clusters of 16 bytes
	* 16 bytes meaning 65,536 numbers instead of just 256 numbers
	* so this time we use hexadecimal out of convenience
* eg. `2001:4860:4806:0:0:0:0:8844`
	* this one belongs to Google
	* since we're quite early into the habit of using IPv6, there probably would be a bunch of 0
		* that seems quite long to repeat the 0s right?
	* sometimes it get shortened like `2001:4860:4860::8844`
___

# DHCP

How do we get an IP address though?
* Certainly we can't just pick the one we want
	* it'll end up somebody fighting us for the same IP address
* Somewhere between our computer and the Internet, there is a DHCP server
	* DHCP, *Dynamic Host Configuration Protocol*
	* all it does is just assign us, our devices, an IP address
* The DHCP server has a list of addresses, it just gives us one, when we connect to the network
	* a bank of available IP addresses
* Before DHCP, it used to be the system administrator's job
	* an actual person would manually assign the computer an address
___

# Internet Protocol
feels like we talked about IP address but not Internet Protocol....
so here we are

Remember we talked about that Internet is a bunch of networks that connect to each other?
Well, it turns out that is not entirely true

Think about it, what it takes to connect, physically I mean, 2 networks, or 2 routers? Cables.
* If we have 6 networks, we would need 5 cables for each routers in order to make each of them connected to others.
* In the real world, it's impossible that we could afford to do this...
	* Like imagine if every router in Asia need to connect to every router in America....
* In a small scale network though, maybe it's worth it

The solution is just, not connect networks to every other networks, just connect to the *important* 1 or 2 of them will do
* router stored a routing table
	* that dictates "where do i go if i'm looking for a particular IP address?"
	* also this type of router may not be the same as the router at our home
* if the IP address starts with `4`, i'm gonna go this way
* if the IP address starts with `12`, i'm gonna go that way
* it's not going to take the envelope exactly to its destination, but one step closer to its destination
	* kinda like the concept of [[CS50x 3-4 Sorting#Recursion|recursion]]
	* then the next router, again, take it one step closer to its destination, then again, and again, ...
	* until it eventually does get to its destination
* then there is that trade off of speed right?
	* surely if routers are connect directly to each other it would just take 1 step to get to the destination instead of taking possibly many many steps until reached destination
	* but surely the cost for doing so is a bigger pain
		* slow connection speed, we can live with that. but bankrupt? meh...

Now we understood how something, some data, get across the Internet, meaning get sent from point A to point B.
However, what if the data is really big?
* that big chunk of data could eat up the bandwidth of a router
* then other data that want to get in to this router, would not be able to get passed
* kinda like if we're driving on the highway, and there is this giant truck blocking the way, the whole highway, then everybody can't move at all
	* unless the car is small enough, small as bicycle

Therefore IP set the rule that there are no big giant track allowed on the highway
* so if a email happens to be so big, it would be splitted out into small packets
* then all these many small pieces would get sent separately

Another advantage of breaking up data is that,
if some routers simply can't keep up to jiggle everything and deliver every packets correctly
then it just drop some, which results in some packets get dropped, failed to reach its destination
* if this packet get dropped happen to be a big one, since we didn't break up the data, we would need to resend it and the highway would be blocked yet one another time
* so it's costly to send big packets, but a lot less costly to send small packets

> [!Summary]
> IP is responsible for:
> 1. getting information from point A to point B
> 2. breaking the information into small pieces so that the network is fair or isn't overly taxed

IP is also known as a *connectionless* protocol
* There is not necessarily a defined path from point A to point B
* There could be more than one ways or one route, to deliver things to an IP address
* It's quite cool right since the networks are more responsive
	* it's like driving on the highway and then the GPS warns us that traffic is ahead, we can just take another route

So router and routing are not just about the routing table, there is more things going on
It also can check the the state of its local network
* Is this path clear? Is that path clear?
* If the highway is completely jammed, just try taking side roads.
	* we're not depending on that one truck moving out of the way

Also it implies that the small packets which originally come from the same big chunk of data
could take different routes to get from the beginning to the end

> [!Summary]
> IP is also a little bit about of preventing traffic in the Internet.

Lastly, what happen if a packet does get dropped?
How could we recover from that?
Actually we're going to need [[#TCP]] for this part.
___

# TCP
*Transmission Control Protocol*

All IP is about, is just transporting information across networks
After a data get into our computers, what do we do with it? IP take no part in this.
It's where TCP comes in.
* It can direct the packet to the correct program, or correct service, on our computers
* So the full story of transporting information invovles
	1. where (network) it's supposed to go. the IP part.
	2. what the packet is for. the TCP part.
* Also often times people just treat them as [[#TCP/IP]]
	* since the 2 protocols are so interrelated
	* but they are 2 separate protocols doing 2 separate things
* The keyword for TCP being, *guarantee delivery*

1. **Port**
* Port number is how a specific service, or utility, or program, is identified on a machine
* So IP address plus port number, give us a particular service running on a particular machine
	* and it implies that IP address alone doesn't do anything

Now let say our computers received a TCP envelope, how does it know it's an email, versus a web page, a Skype call, etc.?

Well the virtual envelope actually contain some more information other than just the addresses.
* Port
* The type of service whose data is in this envelope
* it's represented as an numeric identifier
	* which is standardized across all computers
* placed after the addresses, separated by a colon `:`
	* `<source address>:<port>`, `<destination>:<port>`
	* `80` - HTTP
		* indicate that the envelope contains a web page
	* `443` - HTTPS
		* indicate that the envelope contains a encrypted web page
	* Email could be `25`, `465`, or `587`
* If you've ever had to configure like Outlook or Gmail to talk to another other, you might have seen these numbers
* But we don't need to be too care about these because computers and servers nowadays automate much of this process

> [!Commonly used port number]
> `21` - FTP, *file transfer protocol*
> `25` - SMTP, for e-mail
> `53` - [[CS50x 8-2 Request and Response#DNS|DNS]]
> `80` - HTTP, for web browsing
> `443` - HTTPS, for secure web browsing

2. **Fragmentation**
Okay it will do for sending email.
But what if the message is much bigger than an email?

A email might can fit in to one single packet.
How about sending pictures, or worse, videos?

It could eat up all the bandwidth of the Internet, if the data is large enough
* Every one else who is also sending emails, would need to wait until that picture or video get across the Internet and eventually get to its destination
* We don't want that though. It would be nice if we could kind of time-share the interconnections, across these routers.
	* give a little bit of time to the one sending that really big picture
	* but also give a little bit of time to another person sending a small email
	* a little bit of time for someone who is browsing a web page
* Eventually the really big picture or video get through the Internet. But it doesn't monopolize the bandwidth of the network.

Okay so how computers are doing this?
It's actually one of the features that come with TCP/IP - fragmentation.
* We can temporarily cut a big packet to many small pieces
* and use not just one envelope, but a second, a third, or more

Now if we do this, it feels like we need one more piece of information marked on the envelopes. What is it?
* The order of the envelopes
	* a sequence number, like `1 out of 4`, `2 out of 4`, `3 out of 4`, etc.
	* as a clue to a router, in what order to reassemble these envelopes or these small pieces

Now if we have sequence numbers on the envelopes, what other feature does TCP apparently enabled us to implement?
* It **guarantees** the data will be delivered successfully
* How? If the computer received `1, 2, 4 out of 4` but not the 3rd one, the computer knows it's missing, then just ask the sender to resend that packet one more time.
* Actually this is why, if we receive an email, we either receive the whole thing, or nothing at all.
	* No sentence, word, or paragraph would be missing from an email
	* If we download a photograph from a web, it would not have a blank hole in the middle

* TCP the protocol ensures either all of the information get to its destination, or ultimately, none  of it at all.
	* but if we don't need or even don't want this feature
	* there is another protocol called UDP
		* it doesn't gurantee delivery
	* why we would not want this feature?
		* maybe we're watching a straming video, like a sport event
		* we don't want the thing to buffer, buffer, buffer, buffer, ....
			* we may miss things
			* and then we would be watching a game that ended 20 minutes ago
		* similarly maybe it was a voice call
			* maybe UDP would make it sounds a little crappy
			* at least we can hear them
				* there is no pausing and resending
			* otherwise it would slow down that sort of human interaction
___

# Summary

**IP Address**
* handles the addressing of the packets
* a standardized numbers that every computer gets
	* unique identifier of a computer on a network
* the idea is like the address of our physical home, but it's the address of our computers

**TCP**
* one of the standard ways to send data from point A to point B
___

# TCP/IP Story in a Nutshell

1. I want to send a cat photo to Alice, I hitted the Send button
2. Data get passed to the computer's network software, starting to do all the TCP/IP stuffs

3. TCP breaks the image, the data, to smaller packets
4. then wraps information around each of them
	* that is to say, putting the packets in some envelope and writing some information on it
	* the information indicates
		1. what port is this packet supposed to goes to
		    = what program / service does this packet of data goes to
		2. what order that the packet is out of all

5. IP also gets those packets, then wraps some more information
    (we might call these the IP layers of information, surrounding the packet)
	1. where the packet is supposed to go
	    = IP address of the destination
	2. where the packet is sent from
	    = IP address of the sender, my computer

> [!tip]
> Composite of a packet, is sort of like a nesting doll
> * data < TCP layer < IP layer

6. Then the packet wrapping is done, and the actual sending process begins
7. Other routers doing their things, the [[#Internet Protocol]] thing, to deliver our packets

8. When a packet reached its destination, the machine looks at the IP address and goes "Oh, here is a packet for me, let me see what's inside", then it takes off the IP layer
9. It looks at the TCP layer, and goes "Okay this is a data for port number something, and this packet is the order something out of a total of 10 packets"
10. Then it takes off the TCP layer too, knowing the port number and packet order number, and takes the data inside, and then start preparing the data or reassembling the data

11. Once all packets are received, the whole data is reassembled, TCP hands it off to the correct service, correct port, and goes "Here you go. Here is a data for you."

if
11. one of the packet get lost, who would discover this? TCP or IP? It's TCP.
12. so TCP looks back at the IP layer, to find the sender's IP address, and send a request packet to that address asking for the missing packet
	* surely the packet would tells the missing packet number
	* notice it could lead to some sort of infinite loop if everybody keep losing packets

13. the Internet helps passing the request packet
14. the sender receives it, this time it sends only that missing data instead of the entire image

15. the receiver's TCP gets it, and after TCP reassembled the data, it would pass it along to the correct port
	* where the data will be interpreted as a cat photo
___
