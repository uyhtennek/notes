so far, our web applications only work on our own computer,
but if we want to deploy them to the world, we need to host our web applications
* on some web server

and when we do so, it actually raises a whole bunch of issues
___

# Scalability
the first kind of issues, is around scalability

we deploy our web applications by putting them onto a web server
* it listens for web requests
* then responds accordingly

but, it might have many users that are all trying to connect to our server, at the same time
* then we start to encounter some scalability problems
* because a single computer or a single server, can only service so many users at any given time
* so we need to address that in advance
___

## On-premise / Cloud
so the first question is, where do these servers actually exist?

nowadays, there are two many options
* on the cloud, or
* on premise

On-premise server
* if a company is running their own web application, on-premise servers are servers that are inside of the company's walls
* the company owns the physical servers
	* they probably have a server room
* and therefore, they have very direct control
	* what server do use, what software to run on them, what configurations, etc.
	* they can go and physically look at the servers and debug them, if need be

Cloud computing
* servers that are somewhere in the cloud
	* provided by companies like Amazon, Google, Microsoft, etc.
	* they run the servers, and we simply use the servers
* but we don't have direct control over the machines
	* we can't physically manipulate those computers
* the bright side is that we don't need to deal with the physical objects
	* everything is managed externally by those service-providers
* another good thing is that, it allows web applications to scale across multiple different servers
	* because we might get more and more users
___

## Benchmarking
the second question, how many users can the server service, at any given time?

It varies, based on
* the size of the server
* the computing power of the server
* how long to process a user's request

therefore, we need to do benchmarking
* that is, doing some analysis to figure out how many users a server can handle at a time
* there are numerious tools for that
	* Apache Bench, or known as AB, which is a popular one

but ultimately, it has a finit limit
* every computer just has some finite amount of resources, after all
* servers will eventually go down if there are enough users

so, what can we do in that situation?
___

# Vertical Scaling
the simplest way to scale

if a server isn't big enough, make it bigger
* not two servers, but one server, just bigger

there are drawbacks
* a bigger server still has finite limit
* so eventually, it can reach its limit
___

# Horizontal Scaling
fairly simple as well

if one server isn't enough, try two servers
* but then, if a user sends in a request, how they know to direct it to server A or server B?

we actually need a hardware for this, which we call a, Load Balancer
* when a request comes, it gets to the load balancer first
* then it's directed to Server A or B
* so it's acting as a dispatcher

the process is likely less expensive than actually dealing with and processing requests

and it can go on, maybe we have many servers
the load balancer is just going to balance between all of those servers

the process of deciding which server to send a request to,
is known as *load balancing*
___

## Load Balancing methods

and there are actually some load balancing methods
* Random Choice
	* simply randomly assign the incoming request to one of the servers
	* it has its advantage, it's simple therefore it's quick
	* but it might not be the best option, because we can get unlucky, then the workload is not so balanced amoung servers
* Round Robin
	* request 1 go to server 1, request 2 go to server 2, request 3 go to server 3, ....
	* request 6 go to server 1, request 7 go to server 2, .....
		* so all servers are go through once
	* it's also simple, because it's just keeping count
	* but, if some requests take longer than other requets, and if those longer requests happen to all keep going into the same server, it's unbalanced
* Fewest Connections
	* pick the server which has the fewest connections and assign the incoming request to it
	* it's more like choosing now so it probably can perform better
	* although the computation process is going to be more expensive
		* you can imagine the previous two methods are much easy to implement and execute
* .....

but all of those approaches suffer from another problem,
which has to do with sessions
* recall, we use sessions to store information about the user's interaction, with the web application
* so if the session is stored in the first server that we get to, and then if the second time we make a request but it's not directed to our first server again, it loses our sessions
	* the second server doesn't have our sessions, because it's on the first server

so we need a session-aware way for loading balancing
___

## Session-Aware Load Balancing
well, actually there are multiple ways for this

Sticky Sessions
* the load balancer will remember what server did the user get sent to last time, and send the user there yet again
* again, there are potential risk, if one server ends up serving a bunch of users that are more active than users on the other servers, its workload is gonna be bigger than other servers
	* the users just keep making more requests than others
	* or other users just don't come back, so their servers are less busy

Sessions in Database
* simply store the sessions in database, rather than server
	* and surely all servers need to have access to the database
* so it doesn't matter which server we get sent to

Client-Side Sessions
* store sessions on the client side
* it essentially is cookies
	* ie. set a cookie in the web browser, so the next time it makes a request, it can present that same cookie to our servers
	* we can set a whole bunch of information inside a cookie, including session-related stuffs
* and if it's client-side, surely there would be a risk that people can manipulate that cookie
	* so you might want to do some encryption or some kind of signing
	* so that people can't fake a cookie
* another problem is, if we get so many information into cookies, and they keep getting sent back and forth between the server and client, it can be expensive

.....
* there are other approaches

no one approach is necessarily the right one or the best one, to use
* but we need to be aware of them, because we're start dealing with these problems
* so that we don't break the user experience
___

# Autoscaling

and now, let's say we adapt horizontal scaling,
how many of these servers do we need?

1. you need to do benchmarking so you know how many requests a server can handle at a time
2. you would want to estimate how many users are there for your web applications

but the thing is, the number of users can vary based on time
* maybe sometimes there are going to be far more users than another time

surely you can go for the maximum
* but it probably isn't a great economical choice
* because there might not be so much users for the most time
* yet you're paying electricity for the servers

therefore, one popular solution, especially for cloud computing, is autoscaling
it works like
* we have an autoscalers
* for example it may start with two servers
* if there is enough traffic to the website, maybe it's a peak time, go ahead and scale up
	* add a third server, add a forth server, .....
* if there is not enough traffic, remove a server, remove a second server, .....

* and most autoscalers will let us configure,
	* a minimum number of servers
	* a maximum number of servers

* again, it has its problem
	* this autoscaling process may take time, so we might not be able to service all of the users immediately

* and it introduces opportunities for failure
	* it's still better than having a single server though, because if the one server goes down, the entire web application goes down
		* we generally call that a *single point of failure*
		* a single place, where if it fails, the entire system is broken
	* in the case of autoscaling, if one server goes down, ideally the load balancer should know about that and no longer send requests to that broken server

but how? how the load balancer knows the status of the servers?
one of the most common, is heartbeat
* every so often, every some number of seconds, pings all of the servers, just send a quick requests to all of the servers, to see if any of those not responding back
* then the load balancer can know
	* the latency of each of the servers
	* whether or not a server is functioning

but, you may notice that, now the load balancer appears to be like a single point of failure
* if it fails, nothing is going to work
* and we can solve that basically using the same approach, having more load balancers
___
