# SQL Injection Attacks

often times websites being hacked or passwords leaking out, is because of what are called *SQL injection attacks*

it's made possible by bugs related to how the data is getting into our applications
* eg. Sign In
	* users' email address and password are prompted
	* suppose begin the login page, there's some website
		* which is using SQL to store all of the user's usernames, passwords, ID numbers, email addresses, transcripts, etc.
		* there's a SQL database underneath the website

* so what could went wrong in this process?
	* there's some special syntax in SQL
		* one of which is comment
		* we type two hyphens / dash dash `--` to introduce a comment in SQL
	* sometimes the programmers would do something to defend against potentially adversarial attacks
		* since users are not trust-worthy
		* one common test is to type any username followed by `'--`, and then Sign In
			* eg. `malan.harvard.edu'--`

what would happen if the login code is like this
```python
rows = db.execute("SELECT * FROM users WHERE username = ? AND password = ?", username, password)

if len(rows) == 1:
	# Log user in
```

actually this is fine defending against the `'--` attack
* cs50's SQL library, as well as other third-party libraries, are doing some stuffs with the question mark `?` to defend the attack
* but what if the programmer is constructing the function by himself?
	* perhaps he would use a f-string in Python, just using what's taught to him
	* the code would be like this
```python
rows = db.execute(f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'")
```
* if the value to be formatted is encapsulated by some special syntax like the single quote `''` here
	* you better hope the value doesn't have the same or any special syntax symbol

let's try blindly plug in the user input into it
* what would the Python code be like?
```python
rows = db.execute(f"SELECT * FROM users WHERE username = 'malan.harvard.edu'--' AND password = ''")
```
* what would the SQL command be like?
```sqlite
SELECT * FROM users WHERE username = 'malan.harvard.edu'--' AND password = ''
```

since the SQL command doesn't check for password anymore
the attacker can log in an user's account without knowing its password

this is not even the worst case
imagine the user types `; DROP ...` or `; UPDATE ...`
* eg. `Robert'); DROP TABLE Students;--`
basically if we or attackes can inject SQL into the database, we or they can do anything
___

# Race Conditions

often times we would have multiple users editing the same database
* eg. 2 people trying to log in a website at the same time
* then the computer is trying to handle requests from 2 people at once
* we might get what are called *race conditions*

actually this is not a problem just in SQL or Python, but in computing in general
it happens any time we have shared data

**eg. instagram's likes**
look at any post in instagram, perhaps a post from an account @world_record_egg
ever think of how the likes work?

it's actually a hard problem
* idea is simple though: Simply count the number of likes
* it involves some more steps:
	1. someone clicks on the like button
	2. instagram's code would detect it
	3. instagram's code would update the database
* even though there are multiple people clicking on that same post at the same time

* didn't get what's the hard part?
* let's look at some code, and perhaps the instagram's code works as follow
```python
rows = db.execute("SELECT likes FROM posts WHERE id = ?", id);
likes = rows[0]["likes"]
db.execute("UPDATE posts SET likes = ? WHERE id = ?", likes + 1, id)
```

code like this is not what we'll call *atomic*
* to be atomic means the code will all execute together, or not at all.
* however code typically is executed, *line by line*
	* this is where the problem is
	* for efficiency, the computer, the server, owned by Instagram, might execute line 1 for A user, then line 1 for B user, then line 2 for A user, then line 2 for B user, and so forth.
	* the 2 queries might get *intermingled* (混雜) *chronologically* (按時間序)

but surely we can't do if A user is using Instagram, then block every other users right?
* it's more efficient and fair if the server will do a little bit of work for A, then a little bit of work for B, then C, and back and forth, equitably

long story short, if 2 people click on the like button at the same time
let say the current number of likes is 10
* both users would get the number of likes from database as 10 and thus updating it as 11, if using the above code
* since a user reads the number of likes from the database before another user could update the database

a real life analogy would be
1. 2 people live in the same house
2. A gets home, opens the refrigerator, realizes that there is no milk. So he goes out to buy milk.
3. Meanwhile B gets home, opens the refrigerator and realizes there is no milk. So she too goes out to buy milk.
* both of them made a decision, based on the state of a variable that they independently examined, and they didn't communicate

the solution is obviously, to **communicate**
so if A texted B, the above tragic would be avoided
* or leave a note, or even lock the refrigerator somehow
	* thereby making the milk purchasing process *atomic*
* in CS's words, the solution is letting multiple threads can somehow inter-communicate by having shared state

Actually for years, when social media was first getting off the ground,
this was a super hard problem
* Twitter used to go down all of the time
	* the issues or similar issues happen on tweets or retweets with a very high frequency
___

# Transactions

In programming there is a thing called *lock*
* Software locks allow us to protect a variable so no one else can look at it until we're done with it.
* Transaction is a type of lock
	* allows us to lock out others from accessing the variable, but for slightly less amount of time
* like locking the refriegerator in the roommate analogy

Here is how to introduce a transaction to the above faulty code
```python
db.execute("BEGIN TRANSACTION")
rows = db.execute("SELECT likes FROM posts WHERE id = ?;", id)
likes = rows[0]["likes"]
db.execute("UPDATE posts SET likes = ? WHERE id = ?;", likes + 1, id)
db.execute("COMMIT")
```

the downside though, is
* the more we use transactions, the higher the pobability that someone is blocked out
	* therefore his or her request is a little slower
		* or even make the request fails
	* because people cannot interact with the database at the same time

* so when ever we introduce transactions, make the operations as short and as fast as possible
	* just get in, do the thing, and get out, really fast
	* so as to not cause these kind of performance things
___
