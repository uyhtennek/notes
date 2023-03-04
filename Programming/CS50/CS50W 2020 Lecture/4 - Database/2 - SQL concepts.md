# One Table

recall that our table is like this
| id | origin | destination | duration (in minutes) |
| :-: | :-: | :-: | :-: |
| 1 | New York | London | 415 |
| 2 | Shanghai | Paris | 760 |
| 3 | Istanbul | Tokyo | 700 |
| 4 | New York | Paris | 435 |
| 5 | Moscow | Paris | 245 |
| 6 | Lima | New York | 455 |

however, it doesn't make much sense because, usually a city has more than one airports
* we should use airport codes to represent `origin` and `destination`, instead of cities

surely we can put all of them in one table
| id | origin | origin_code | destination | destination_code | duration (in minutes) |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 1 | New York | JFK | London | LHR | 415 |
| 2 | Shanghai | PVG | Paris | CDG | 760 |
| 3 | Istanbul | IST | Tokyo | NRT | 700 |
| 4 | New York | JFK | Paris | CDG | 435 |
| 5 | Moscow | SVO | Paris | CDG | 245 |
| 6 | Lima | LIM | New York | JFK | 455 |

that doesn't make much sense either
* table is starting to get wide, making it hard to navigate
* there are duplicated data
	* eg. Paris and CDG are always associated, why would we write both of them down
___

# Relational Database

we solve these problems by having another table, because
* flight is one type of object, that deserves a table on its own
* airport is another type of object, that deserves another table

`airports`
| id | code | city |
| :-: | :-: | :-: |
| 1 | JFK | New York |
| 2 | PVG | Shanghai |
| 3 | IST | Istanbul |
| 4 | LHR | London |
| 5 | SVO | Moscow |
| 6 | LIM | Lima |
| 7 | CDG | Paris |
| 8 | NRT | Tokyo |

then what happen to our `flights` table?
* instead of storing texts, the city names
* we store a foreign key
	* which is a reference to a key in another table, the `airports` table

`flights`
| id | origin_id | destination_id | duration (in minutes) |
| :-: | :-: | :-: | :-: |
| 1 | 1 | 4 | 415 |
| 2 | 2 | 7 | 760 |
| 3 | 3 | 8 | 700 |
| 4 | 1 | 7 | 435 |
| 5 | 5 | 7 | 245 |
| 6 | 6 | 1 | 455 |

and you can imagine this ability of relating tables to tables, is incredibly powerful
eg. we can make another table for the passengers
| id | first | last | flight_id |
| :-: | :-: | :-: | :-: |
| 1 | Harry | Potter | 1 |
| 2 | Ron | Weasley | 1 |
| 3 | Hermione | Granger | 2 |
| 4 | Draco | Malfoy | 4 |
| 5 | Luna | Lovegood | 6 |
| 6 | Ginny | Weasley | 6 |
___

# Designing a Table

when we're creating our tables, we need to be ware of its implication
* eg. for the `passengers` table, any row can only be associated with one `flight_id`
	* in this system, it's impossible that one person can get on multiple flights
	* it's okay because it makes sense

**many-to-one relationship**, or **one-to-many relationship**
* eg. one flight can be associated with many dfferent passengers

**many-to-many relationship**
* eg. many different passengers can be associated with many different flights
	* one flight can have many passengers
	* one passenger can be on many flights
___

## many-to-many structure

`people`
| id | first | last |
| :-: | :-: | :-: |
| 1 | Harry | Potter |
| 2 | Ron | Weasley |
| 3 | Hermione | Granger |
| 4 | Draco | Malfoy |
| 5 | Luna | Lovegood |
| 6 | Ginny | Weasley |
* we just going to need information about a person in our `people` table
* so no flight information

and then we'll have a separate table for dealing with passengers on the flight
* this table will do the mapping, between passengers and flights
* therefore this table needs a foreign key that references the `flights` table, and another foreign key that references the `people` table

`passengers`
| person_id | flight_id |
| :-: | :-: |
| 1 | 1 |
| 2 | 1 |
| 2 | 4 |
| 3 | 2 |
| 4 | 4 |
| 5 | 6 |
| 6 | 6 |

* it's known as an *association table*, or a *joined table*
* eg. in the first row, it states that the person with an id of 1, is on the flight with an id of 1
	* then we can look up to the two tables to figure our who the person is and what the flight is

* notice that, `person_id` 2 is on both flight 1 and 4
* flight 6 has two passengers, they are person 5 and 6

The by-product is that, the table is a little messy
* we need to look up to different tables, in order for us to draw any conclusion
___

# `JOIN`

SQL has a way for us to tidy up the messiness a bit
* by joining different tables together

```sqlite
SELECT first, origin, destination
  FROM flights JOIN passengers
    ON passengers.flight_id = flights.id;
```

the result looks like this
| first | origin | destination |
| :-: | :-: | :-: |
| Harry | New York | London |
| Ron | New York | London |
| Hermione | Shanghai | Paris |
| Draco | New York | Paris |
| Luna | Lima | New York |
| Ginny | Lima | New York |


## `JOIN` types
there are different ways to `JOIN`

`JOIN` / `INNER JOIN`
* the default `JOIN`
* take two tables, cross compare them based on the condition specified, and return back the results only if there is a match

`LEFT OUTER JOIN`
`RIGHT OUTER JOIN`
`FULL OUTER JOIN`
* if we're okay that there isn't a match, but we still want to obtain the information from either one table or both tables, we can use the outer join
___

# `CREATE INDEX`

we can make optimizations, to make queries more efficient
* we can do that by creating an index on a particular table

it's much like the index page of a book
* it helps us quickly find a topic in a book, as opposed to search page by page
* note that it has nothing to do with the content of the book
	* it's additional, not important for serving information
	* all it does is just making the book easier to navigate
	* yet it takes some pages

similarly, an index on a table, is additional
* likewise, it does take time and memory to construct and maintain, any time we update the data inside the table
* but once it exist, it makes querying on that table much more efficient

```sqlite
CREATE INDEX name_index ON passengers (last);
```
* we expect, as we query this table, we'll be pretty frequently looking up passengers by their last name, so we create an index on that table based on their last name
___

# SQL Risks


## SQL Injection
there are potential threats associated with SQL, Injection being one of the famous ones

we have a table for users, which has a column of username and a column of password
then we would use something like this to look up and verify a user
```sqlite
SELECT * FROM users
 WHERE username = "harry" AND password = "12345";
```
* our logic might be, if we did find a user by this query, the user is verified, so sign the user in

then what happen if we have a query like this?
```sqlite
SELECT * FROM users
 WHERE username = "hacker"--" AND password = "";
```
* it turns out `--` in SQL stands for a comment
* so the user here bypassed the password check

### solution

1. first solution would be escaping the characters
* `\"hacker\"\-\-`

2. another strategy is to use an abstraction layer on top of SQL
* so we don't need to worry about the syntax of SQL at all


## Race Conditions

it happens anytime we have multiple events that are happening simultaneously
* eg. on social media like Instagram or Twitter, when two people like the same post at the same time, unexpected things happen

### solution

1. lock the database when necessary
* "I'm working on this database, and I want nobody else to touch the data."
* after the transaction is finished, when all changes that it needs to make finished, when it's done, we can release the lock and let someone else go ahead to modify the database
___
