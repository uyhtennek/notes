# Relational database

* we have play around CSV file a few times already
	* and CSV files are [[CS50x 7-1 Playing with Data#Data collection as CSV|flat-file databases]]
	* simple flat file, no hierarchy. just rows and columns storing ASCII or Unicode text.

* a relational database though is closer to a proper spreadsheet program
	* CSV is an individual sheet from a spreadsheet
		* if we had multiple sheets (= tables) in a spreadsheet, we would have to export multiple CSV files
		* but it's annoying in code because we would have to `open()` multiple CSV files although all of them come from the same spreadsheet
	* to play around with a relational database, it's a very different story than playing with CSV
	* we actually can interact with it, much like how we would use Google Sheet / Excel / Numbers
		* but through code, instead of keyboard and mouse
		* or specifically a programming language called SQL

* okay, but can't we just use spreadsheet programs?
	* if we open up a spreadsheet that got tens of thousands of rows, it'll probably work fine
	* how about hundreads of thousands of rows, or millions of rows?
	* no way. at some point the computer will struggle to open large data sets.
* at that point, relational databases and SQL come into play
___

# CRUD
SQL really just do 4 things fundamentally.
but it does those 4 things really well.

**C** - create data
**R** - read data
**U** - update data
**D** - delete data

So commands in SQL can just categorized in 4 types
```sqlite
CREATE, INSERT
SELECT
UPDATE
DELETE, DROP
...
```

eg. To create a new sheet / table
* something like clicking the + button in Google Sheets / Excel / Apple Numbers
* `CREATE TABLE table (column type, ...);` is the command we're looking for
	* `table` is the name of the sheet / table
	* but we have to decide what type of data we're going to be storing in each of these columns
		* it's strongly typed because it allows better performance / faster operations
___

# `sqlite3`

there are many different tools and softwares that implement the SQL language
* MySQL
	* from Microsoft
	* popular for years
* PostgreSQL
* Microsoft Access Server, Oracle, etc.

SQLite is a lightweight version of the SQL language
* generally used on iPhones and Android devices these days
	* for example if we download an app that stores data like our contacts
		* typically it's stored using SQLite
	* because it's fairly lightweight but still can store thousands, tens of thousands of pieces of data
* SQLite3 is like version 3 of this tool
___

# Create a database

* To convert a csv file to a relational database file
1. run `sqlite3 <filename>.db`
	* it creates a relational database file and get us inside the `sqlite3` program
		* or it'll open the file instead of create a new one, if that file exists
	* it's conventional to name file created by `sqlite` something .db
		* stands for *database*
2. enter `.mode csv`
3. enter `.import favorites.csv favorites`
	* now `favorites` is a table storing the same data from `favorites.csv`

* To see what the schema is of the data
	* schema = design = structure
1. simply enter `.schema`, then it would give, by default, this
```sqlite
CREATE TABLE IF NOT EXISTS "favorites"(
	"Timestamp" TEXT,
	"title" TEXT,
	"genres" TEXT
);
```
this syntax essentially is what the `.import favorites.csv favorites` command did from the above example

`Q:` Can we convert the table back to CSV?
`A:` Yes. There is a command within SQLite that allows that.
Generally though, once we're in the world of SQL, the data probably would be there long term.
Maybe just updating it, deleting it, adding to it, etc.

ps. to quit the `sqlite3` program
```sqlite
sqlite> .quit
```
___

# `SELECT`

now say we want to select all of the titles
* [[CS50x 7-1 Playing with Data#Cleaning data|doing so in Python]] requires opening the file, creating a reader, iterating over every row, ...
* in SQLite it's just a command `SELECT title FROM favorites;`, the result is like
	* a simple `SELECT` command is like `SELECT columns FROM table;`
```sqlite
sqlite> SELECT title FROM favorites;
+-----------------------------+
|            title            |
+-----------------------------+
| How i met your mother       |
| The Sopranos                |
| Friday Night Lights         |
| Family Guy                  |
| New Girl                    |
| Friends                     |
| ....                        |
+-----------------------------+
```
the output is very textual, but it does create a some what friendly graphical table for us

remember we would have write dozens line of code in Python to do just this
but in SQL it'd get much simpler
* since SQL is optimized for CRUD operations

the above example selects one column only 
here is how to select multiple columns at once
```sqlite
sqlite> SELECT title, genres FROM favorites;
```
however the graphical table probably won't look nice

and finally here is how to select every columns
```sqlite
sqlite> SELECT * FROM favorites;
```
___

# Modifiers

similar to Excel or Google Sheets, which provides a feature named *formula*
SQL provides *modifiers* that we use to apply some operations on an entire column

here are some of them
```sqlite
AVG
COUNT
DISTINCT
LOWER
MAX
MIN
UPPER
...
```

* `DISTINCT`
   it removes duplicates
```sqlite
sqlite> SELECT DISTINCT(title) FROM favorites;
```

notice that [[CS50x 7-1 Playing with Data#Cleaning data|formatting]] is an issue again
* so we have to deal with whitespaces and capitalization again
	* by altering the command a bit
	* like `SELECT DISTINCT(UPPER(title)) FROM favorites;`
* except there are limitations for doing something like these in SQL
	* so this time Python probably can do a better job
	* there is this trade-off

* `COUNT`
   it counts something
```sqlite
sqlite> SELECT COUNT(title) FROM favorites;
+--------------+
| COUNT(title) |
+--------------+
| 158          |
+--------------+
```
___

# Filters
it's like the [[C - 3 operators#Boolean Expressions|boolean expressions]] in C or Python, but this time it's in SQL

```sqlite
WHERE
LIKE
ORDER BY
LIMIT
GROUP BY
...
```

* `WHERE`
	* filter the data where it's something is true
	* and filter out the data where that's false
* `LIKE`
	* allows us to do approximations
	* filter the data that kind of like `The Office` but not exactly typed as `The Office`
		* so something like `Office`, `the office`, `theoffice` still can fall in the filter
		* to do so we again use pattern matching together with this `LIKE` command
			* but this time it's not using regular expression
			* in fact the notion of pattern matching in SQL is much more limited
```sqlite
sqlite> SELECT title FROM favorites WHERE title LIKE "office";
+--------+
| title  |
+--------+
| Office |
| Office |
+--------+
sqlite> SELECT title FROM favorites WHERE title = "office";
sqlite> SELECT title FROM favorites WHERE title LIKE "%office%";
+--------------+
|    title     |
+--------------+
| Office       |
| Office       |
| The Office   |
| The Office   |
| The Office   |
| The Office   |
| The Office   |
| Thevoffice   |
+--------------+
```
* as oppose to `LIKE`, we can use `=` to check for equality
	* `SELECT title FROM favorites WHERE title = "office";`
	* `=` in SQL means equality, not assignment
		* not like other programming languages
* percent sign `%` here means 0 or more characters
	* in this case it's 0 or more characters on the left and on the right

* `LIMIT`
	* limits the number of results
```sqlite
sqlite> SELECT title FROM favorites LIMIT 5;
+-----------------------------+
|            title            |
+-----------------------------+
| How i met your mother       |
| The Sopranos                |
| Friday Night Lights         |
| Family Guy                  |
| New Girl                    |
+-----------------------------+
```
___

# Benefit of SQL

say if we want to delete every row that its `title` is `Friends` from the table `favorites`
```sqlite
sqlite> DELETE FROM favorites WHERE title LIKE "%friends%";
```

just like that, Friends are deleted from the database, in **real time**
and actually this is another compelling thing about relational database and SQL
* although we can do the same thing using any programming language
	* like in Python we can open the file with flag `"A"` for append or `"W"` for write, instead of `"R"` for read alone
	* but it's a little more steps involved
* if we were actually running a web application or mobile app correlated to the database
	* any changes to the database would be reflected everywhere in real time
		* surely it assumes the database is somehow "talking" to the applications

same goes for updating data
```sqlite
-- change "Thevoffice" to "The Office" --
sqlite> UPDATE favorites SET title = "The Office" WHERE title = "Thevoffice";

-- force the 'genres' of every "Game of Throne" row --
sqlite> UPDATE favorites SET genres = "Adventure, War" WHERE title = "Game of Throne";
```

* but **beware** using `DELETE`, and worse `DROP`
	* whereby we can `DROP` an entire table
* and **beware** there isn't an undo command or time machine with a SQL database
	* if we don't have a back-up, it's doomed
___

# Capitalization in SQL

convention is to capitalize all of the special SQL keywords
and lowercase all of the column names and the table names

just help readability

```sqlite
1. SELECT genres FROM favorites;
2. select genres from favorites;
```
they are the same
___

# SQL data types
there is 5 and only 5 data types

```sql
BLOB      -- binary large object, it's just raw 0s and 1s
--        -- for files or things like that

INTEGER   -- integer, positive or negative

NUMERIC   -- dates and times
--        -- or anything thing that is numeric but not just integer or real number

REAL      -- real number, or float if it was other programming language

TEXT      -- just text, but we don't need to worry about its size
--        -- like in Python, it will size to fit
```
___
