server isn't the entire story,
because we still have data to deal with
___

# Database Server

SQLite, that we used, just stores data inside of a file
* but it's quite common that we would want to put databases entirely somewhere separate
	* have a separate database server running somewhere else
* then have the servers communicating with the database

much like how servers have their finite limit,
database also suffer from having finite resources
___

# Database Partitioning

splitting up a big data set, into multiple different parts
* so that it's faster to query

actually we've already seen one example of that
* we have an `airports` table like
| id | code | city |
| :-: | :-: | :-: |
| 1 | JFK | New York |
| 2 | PVG | Shanghai |
| 3 | IST | Istanbul |
| ... | | |
* then we also have a `flights` table
| id | origin_id | destination_id | duration |
| :-: | :-: | :-: | :-: |
| 1 | 1 | 4 | 415 |
| 2 | 2 | 7 | 760 |
| 3 | 3 | 8 | 700 |
| ... | | | |

* as opposed to having one big table, with more columns
* we split it up into more tables but fewer columns
	* we would call it, *vertical partitioning* of a database

so, it implies that we also have *horizontal partitioning*
* we take a table, and we split it up to multiple tables, but they are all storing effectively the same data, just stored in different data sets
	* so the same type of data, but just in different tables
* eg. we originally have a `flights` table, and we split it up to a `flights_domestic` table and a `flights_international` table
	* they have the exact same columns
* the advantage is that, if we are looking for one domestic flight, we don't need to look through the massive `flights` table
	* if you know you are searching for an domestic flight, the search can be more efficient
* but the disadvantage is that, it's more expensive if we ever need to join this data back together
	* so try to separate the data in a way that, we generally need only dealing with one table but the multiple tables
___

# Database Replication

again, we want to avoid single point of failure
* so, to solve it, we replicate our database
* there are a couple of ways to do that, we'll talk about two of them

Single-Primary Replication
* we have multiple different databases, but only one of them is the primary one
* we can read from all databases, but only can write to that one primary database
	* meaning we can only do insert, update, delete on the primary database
* but how do we make sure that all databases are kept in sync?
	* it's simple, any time we write something to the primary database, it informs other databases to update
* then the drawback becomes, if we need to do many write operations, we have only one database for that
	* the primary server is carrying all of the load
* and it still suffer in single point in failure
	* if the primary database fails, we can't do writing

Multi-Primary Replication
* so this time, every database can do both write and read
* but now the sync is trickier
	* any writing performed on any one database, need to be informed to any other databases
	* and it introduces the possibility for conflicts
		* eg. a update conflict, ie. two databases trying to update the same row
			* then when they try to sync, we need to have some way to decide which version to use, or how to resolve the conflict
		* eg. uniqueness conflict, like if we insert the same row in two databases, both are given the same unique ID
		* eg. delete conflict, one person try to delete a row, but another person try to update it

* as a result, we need to design more sophisticated system, that are able to deal with those issues
___

# Caching

Ultimately, we'd like to reduce the number of database servers that we have
* because every additional database server is going to cost time, resources, money
* so we want to talk as least as possible to the databases
* eg. you can imagine developing a website for New York Times, we need to query the database for all of the recent top headlines and the corresponding information
	* yes it can work. but how about we have a bunch of people visiting the homepage at the same time?
	* it doesn't make much sense because, we don't really need to do the query per user
		* since the articles might not be changing
		* the recent top headlines are not going to change in a second or two, but in days
	* so maybe we can skip the process of querying the database and re-generating the template, for every single user
		* how? we can cache the template.

In general, caching means, storing a saved version of some information, in a way that we can access it more quickly
* so that we don't need to keep making requests to the database

we have a number of ways to do caching
___

## Client-Side Caching
cache data to users' browsers

eg. you visited a page, and requested an image
* then you reload the page, your web browser might try and make a request, for the same image
* an alternative would be, your web browser just save a copy of the image inside of a cache, locally
	* so the next time you visit the page, it loads the image locally instead of remotely
	* so that it doesn't need to talk to the server at all

it's a quite good of a solution because
* it's fast for the users, because they don't need to wait for the server to respond
* it offloads some work for our server, because there are less requests to deal with

how to cache things though?
we need to add something to the HTTP response
```
Cache-Control: max-age=86400
```
* it tells the browser to cache this resource for `86400` seconds

however, what if the resource changes within this `86400` seconds?
the user would still looking at the cached, in this case outdated, version of the web page
* this might be the case for a web page
* it's especially true for static resources
	* eg. CSS file, JavaScript file
	* you might have already experienced this, when working on your own web applications
		* you changed some CSS and hit refresh, yet the changes are not reflected
		* that's because your web browser is caching those results
	* so, in most web browsers, we can do a hard refresh
		* then the web browser would ignore whatever is in the cache, and make a new request to get some new data
		* although, by default, our web browsers prefer the cached version of data

despite this is quite a popular strategy already, we can add to this by adding a ETag
```
Cache-Control: max-age=86400
ETag: "7477656E74796569676874"
```
* a ETag is just some unique sequence of characters, that identifies a particular version of a resource, like a CSS file, JavaScript file, etc.
	* when our browser requests a resource, like the CSS file or the JavaScript file, it can the associated ETag value as well, so a ETag identifies a version of the resource
	* then if the web server were ever to change that CSS file, the corresponding ETag will also change
	* then when our browser makes new requests, it can check the ETag value first, before actually trying to request a resource, and decide whether or not it should use the cached version
		* based on whether or not the ETag value is different
		* and checking that is a short sequence that can be answered very quickly

1. the client sends a request with an ETag to the server, if it has a cached version of the resource
2. if the ETags are the same, server responds saying, "okay go ahead and use your cache"
3. if the ETags are different, server sends the resource to the client along with the new ETag and tells it to update the ETag

in addition, cache time and ETag can work together
* servers can tell clients to, "go ahead and cache this resource for some number of seconds", so that for some number of seconds, a client is not ever going to request a new version of the resource
* but even after this some number of seconds have passed, if the ETag didn't change, then the clients don't need to redownload a whole new version of the resource
	* because there is none
	* just keep reusing the cached version on the client's browser
___

## Server-Side Caching

so we have servers communicating with the database,
but we can also have the servers to communicate with the cache
* which is some place that is storing the information that we might want to reuser later

Django turns out have an entire cache framework
* again, we don't need to do much, Django gives these features to us for free

Per-View Caching
* we can specify a cache on a particular view
* so, rather than run through all these Python code in a view whenever someone made a request, we can just cache the view
* so that, for the next 30 seconds or 30 minutes or something, the next time someone tries to visit the same view, we can just reuse the results of the last time that that view was loaded

Template Fragment Caching
* similar as above, but this time, rather than applying to a view, it's applied to fragments inside of a template
* our template might have multiple different parts
	* eg. on a web page, it might have the navigation bar, the sidebar, the footer, etc.
	* maybe based on some information about today, they might change the next day, but they might not change very often within the same minute or the same hour
	* then maybe we want to cache the non-changing parts of a template
* so that the next time Django try to render a template, it doesn't have to recalculate the sidebar, or the navigation bar, or whatever cached
* it just uses the cached version of the sidebar or the navigation bar or whatever
	* as they are loaded from the last time Django renders a template

Low-Level Cache API
* maybe there is an expensive database query, that takes a couple of milliseconds or a couple of seconds to process
* we can save the result inside of a cache
* so it's easy to access that same data, so we can skip the query if the data is requested
___
