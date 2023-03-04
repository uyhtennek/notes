SQL is a database language
* used to interact with databases

Models are Python classes and objects
* they are what we interact with in Django in order to interact with the databases

Migrations is a technique, that allow us to update our databases
* we perform migrations on models
___

# Data
surely we're talking about relational database here

eg. we're going to build a database and web application that an airline might use
* keeping track of different flights
* keeping track of which passengers are on those flights

we have a table that keeps track of flights
* one row is one flight
| origin | destination | duration (in minutes) |
| :-: | :-: | :-: |
| New York | London | 415 |
| Shanghai | Paris | 760 |
| Istanbul | Tokyo | 700 |
| New York | Paris | 435 |
| Moscow | Paris | 245 |
| Lima | New York | 455 |
___

# SQL

it's a database language
* designed to interact with *relational databases*
	* databases that organize data into different tables

common *database management systems* are
* MySQL, PostgreSQL, SQLite
* ....

usually big companies have separate servers for the web application and the database
* so they are independent
* we can debug and test them independently
* if one goes down, the other doesn't necessarily go down

* therefore MySQL and PostgreSQL allow listening for requests and making responses
	* but SQLite doesn't

SQLite is just going to store all of its data as a single file
* so it's friendly to beginner
* in addition, MySQL, PostgreSQL, SQLite share many SQL syntax
* also, Django's default is SQLite
___

## SQL data types

SQLite has a short list of basic types
* `TEXT`, `NUMERIC`, `INTEGER`, `REAL`, `BLOB`
* `NUMERIC` is much different from `INTEGER` and `REAL`
	* some numeric data aren't really interger or real number
	* eg. Boolean value, date, ....
		* so basically, numeric data that isn't `INTEGER` or `REAL`, is `NUMERIC`
* `BLOB` stands for *binary large object*
	* it's any type of binary data, just zeros and ones
	* eg. files, audio files, images, ....

MySQL supports
* `CHAR(size)`
	* `TEXT` in SQLite can be any length
	* `CHAR` in MySQL takes an argument of `size`
		* sometimes we know that the data is going to be within a certain number of characters
		* eg. ZIP code, in United States, it's always just five characters
	* it's more efficient
* `VARCHAR(size)`
	* meaning, variable length character
	* so `CHAR(size)` means the data is exactly in `size` length
	* `VARCHAR` means the data can be up to `size` length
* `SMALLINT`, `INT`, `BIGINT`
	* each of them uses a different number of bytes to store an integer data
* `FLOAT`, `DOUBLE`
	* like C, `DOUBLE` uses more bytes of memory than `FLOAT`
* ......
___

# SQL Queries


## `CREATE`

```sqlite
CREATE TABLE flights (
	id INTEGER PRIMARY KEY AUTOINCREMENT,
	origin TEXT NOT NULL,
	destination TEXT NOT NULL,
	duration INTEGER NOT NULL
);
```

1. `CREATE TABLE`
* meaning, "I would like to create a new table"

2. `flights`
* the name of the table

3. inside the parentheses, is a comma-separated list
* of all of the columns in the table

The table doesn't have any data yet
* the `CREATE TABLE` query is just gonna decide the structure of the table
	* what are the columns?
	* what are the columns' types?

4. what are the columns?
* `id / origin / destination / duration`
* they are the names of the columns

* having an `id` column is helpful because it gives us some way to uniquely referencing an element
	* eg. maybe there are multiple flights that are flying from New York to London, and usually we would want some way to distinguish them
	* one easy way, is to give every single entry inside of a table, a unique identifier
		* that's the `id` column

5. what are their types?
* `INTEGER / TEXT / TEXT / INTEGER`

6. we can have additional constraints for the columns
* `PRIMARY KEY`
	* it's means the `id` column will be the primary way to uniquely identify a flight
	* then there probably is some optimizations here to make it very quick to look up a flight
* `NOT NULL`
	* meaning, "I don't want to allow this column to ever be empty"
	* in this case, we don't want there to ever be a flight that doesn't have an `origin` or `destination` or `duration`

7. `AUTOINCREMENT`
* it's a cue into SQL to automatically update the `id` every time we add a new row into the table
* eg. if we add a new flight, we need to give it an `origin`, `destination`, and `duration`
	* but not the `id`
	* because SQL is going to give it an `id` automatically


### Constraint types

`NOT NULL`, `PRIMARY KEY`
* explained

`DEFAULT`
* set a default value

`UNIQUE`
* guarantees that every value is going to be unique
* don't allow the same value to appear twice for a particular column

`CHECK`
* it makes sure the value obeys a certain condition
* eg. a number falls within a certain range
	* eg. movie ratings, allow only 1 to 5 or 1 to 10 or whatever range

.....

> [!info] Constraint
> It's used to ensure that, as we add data to the table, the data is going to be valid in some way. It needs to obey the constraints we set. Otherwise, if we try adding an invalid data to the column, it's not going to be accepted at all.

___

## `INSERT`
insert data into a table

```sqlite
INSERT INTO flights
	(origin, destination, duration)
	VALUES ("New York", "London", 415);
```

1. `INSERT INTO`
* "I would like to add a new row, into the table"

2. `flights`
* the name of the table

3. `(origin, destination, duration)`
* the names of the columns that we are going to provide values
* "I would like to provide values for the columns `origin`, `destination`, and the `duration`."
	* note that we don't have to worry about the `id` column, because it's autoincrement
	* the id is automatically given, in this case

4. `VALUES ("New York", "London", 415)`
* after the word `VALUES`, are the values that we provide to the columns, in the same order
___

## `SELECT`
get data out of a table
* not modify data, just retrieve data

1. select all
```sqlite
SELECT * FROM flights;
```
* \* is a wild card that means 'everything'
	* technically it means 'all columns'
* so it's "select everything or all columns from the table `flights`"

2. select columns
```sqlite
SELECT origin, destination FROM flights;
```
* select only the columns `origin`, `destination`
* because many times we actually don't care all of the columns, but we just care about a few
	* it would be wasteful to select everything if we don't need that much

3. select rows
```sqlite
SELECT * FROM flights WHERE id = 3;
```
* here we only want the rows where the `id` is `3`
* the system will look through the table, finding the row where the id is equal to 3
	* we will just get back that one row

* similarly, we can select by not just the column `id`, but any column
```sqlite
SELECT * FROM flights WHERE origin = "New York";
```
___

# SQLite
how to use SQLite?

1. since SQLite database is just a file, we start from creating the file
* in the terminal, execute
```bash
touch flights.sql
```
* `touch` is a command for creating a file
* by default, `.sql` file has nothing in it

2. enter the SQLite prompt, by running
```bash
sqlite3 flights.sql
```
* this is where we run SQL commands

3. create a table
```sqlite
CREATE TABLE flights (
	id INTEGER PRIMARY KEY AUTOINCREMENT,
	origin TEXT NOT NULL,
	destination TEXT NOT NULL,
	duration INTEGER NOT NULL
);
```
* created a table called `flights`

* we can type `.tables` to see all the tables that exist in our database
* in addition, we can use `SELECT * FROM flights;` to see what's in the table
	* apparently it has nothing, because we didn't add the rows yet

4. add a row
```sqlite
INSERT INTO flights (origin, destination, duration) VALUES ("New York", "London", 415);
```
* again we can use `SELECT * FROM flights;` to see if the operation succeeded

* we can copy commands from file outside of SQLite prompt and then paste them in
	* we don't have to type everything

5. usually SQLite doesn't display the selection in a nice format, we can choose to use other formats
```sqlite
.mode columns
.headers yes
```

6. play around
```sqlite
SELECT * FROM flights WHERE origin = "New York";
```


## Boolean expressions
here we're just toying with selections

```sqlite
SELECT * FROM flights WHERE duration > 500;
```

```sqlite
SELECT * FROM flights WHERE duration > 500
	AND destination = "Paris";
```

```sqlite
SELECT * FROM flights WHERE duration > 500
	OR destination = "Paris";
```

```sqlite
SELECT * FROM flights WHERE origin IN ("New York", "Lima");
```
* select any row that has an `origin` of `"New York"` or `"Lima"`

```sqlite
SELECT * FROM flights WHERE origin LIKE "%a%";
```
* `%` is a wild card, that is looking for **0 or more characters, no matter what it is**
* `"%a%"` is looking for values that have any number of letters, including no letter at all, before and after the letter `a`
	* ultimately it's looking for values that have the character `a` in it
___

# `SELECT` Functions

we can have additional functions for our `SELECT` queries
* instead of just selecting the values of columns, or the rows

`AVERAGE`
`COUNT`
`MAX`
`MIN`
`SUM`
....
___

# More SQL Queries


## `UPDATE`

```sqlite
UPDATE flights
	SET duration = 430
	WHERE origin = "New York"
	AND destination = "London";
```

1. `UPDATE`
* "I would like to update a table"

2. `flights`
* the table to update

3. `SET duration = 430`
* "I would like to set the `duration` to `430`"
* but where you want to make the changes?
	* surely we don't want to change `duration` to `430` for every single row

4. `WHERE origin = "New York" AND destination = "London"`
* we perform the change for the rows that satisfy this where clause
___

## `DELETE`

```sqlite
DELETE FROM flights WHERE destination = "Tokyo";
```
___

# More clauses
other than `WHERE`

`LIMIT`
* limit the results to come back

`ORDER BY`
* order the results that come back, by a column

`GROUP BY`
* group a bunch of rows together, by a column
* usually used along with functions like `AVERAGE` or `COUNT`

`HAVING`
* this is a constraint we can place on `GROUP BY`
* eg. "I would like to group flights by their `origin`, but they **must have** at least a count of at least three", meaning there must be at least 3 flights that are leaving from an `origin` to be selected

....
___

> [!info] SQL and Django
> However, many of those SQL syntax, we don't write them ourselves.
> because we write Python, and let Django manipulates the SQL database for us.

___
