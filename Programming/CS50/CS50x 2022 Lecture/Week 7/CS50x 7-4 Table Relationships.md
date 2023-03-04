# Limitation of CSV

recall [[CS50x 7-1 Playing with Data#Data collection as CSV|how]] the Google Sheet store genres of a TV show in a single column
* it's actually quite hard to get all the shows that are in the genre `'Comedy'`, or any other genres
* if we use `SELECT genres FROM favorites WHERE genre = "Comedy";`
	* TV shows that are categorized as `"Comedy, Drama"` would not be included
	* we would need `SELECT genres FROM favorites WHERE genre = "Comedy" OR genre = "Comedy, Drama" OR genre = "Comedy, News, Talk Show";`
	* or better, we can use `SELECT genres FROM favorites WHERE genre LIKE "%Comedy%";`

* perhaps the problem of searching for `"Comedy"` was fixed. how about if we have another 2 genres `"Music"` and `"Musical"`, and this time we're searching for `WHERE genre LIKE "%Music%"`?
	* we probably still can hack a solution in some way
	* but it's not gonna be a good design any more

* the folks at Google did this format probably just because it's simpler
	* the only special character in a CSV is the comma `,` alone
	* but there is a better way to do the categorization in the world of SQL
___

# Table Relationships

Perhaps the better solution would be reading the `'title'` and `'genre'` to 2 seperate sheets
and we would do this in Python
```python
# favorites8.py
# Imports titles and genres from CSV into a SQLite database

# we use the cs50 library here not for get_string or the like
# we use it this time for its SQL functionality
import cs50
import csv
  
# Create database
open("favorites8.db", "w").close()
db = cs50.SQL("sqlite:///favorites8.db")
  
# Create tables
db.execute("CREATE TABLE shows (id INTEGER, title TEXT NOT NULL, PRIMARY KEY(id))")
db.execute("CREATE TABLE genres (show_id INTEGER, genre TEXT NOT NULL, FOREIGN KEY(show_id) REFERENCES shows(id))")
  
# Open CSV file
with open("favorites.csv", "r") as file:
	
    # Create DictReader
    reader = csv.DictReader(file)
    
    # Iterate over CSV file
    for row in reader:
	    
        # Canoncalize title
        title = row["title"].strip().upper()
        
        # Insert title
        show_id = db.execute("INSERT INTO shows (title) VALUES(?)", title)
        
        # Insert genres
        for genre in row["genres"].split(", "):
	        db.execute("INSERT INTO genres (show_id, genre) VALUES(?, ?)", show_id, genre)
```

^77a6be

next when we enter the `sqlite3` program with `favorites8.db` opened
then we enter `.schema`, we can see
```sqlite
sqlite> .schema
CREATE TABLE shows (id INTEGER, title TEXT NOT NULL, PRIMARY KEY(id));
CREATE TABLE genres (show_id INTEGER, genre TEXT NOT NULL, FOREIGN KEY(show_id) REFERENCES shows(id));
```

here we can see, the file `favorites8.db` contains 2 tables
* one is called `shows`
	* which has 2 columns
		* one is called `id`; one is called `title`.
		* `id` is an `INTEGER`; `title` is `TEXT` but it can never be `NULL`.
	* there is also a `PRIMARY KEY`, specifically to be the `id` column
		* A `PRIMARY KEY` is the column that uniquely identifies all of the data
		* therefore every piece of data is unique
* another table is named `genres`
	* first column is `show_id`; second is `genre`.
		* again, one is an `INTEGER`; another one is a `TEXT NOT NULL`.
	* there is also a `FOREIGN KEY`, specifically to be the `show_id` column that `REFERENCES` `shows(id)`
		* A `FOREIGN KEY` is the appearance of another table's `PRIMARY KEY` in its own table
		* in our case `show_id` is a "foreign" version of `id` in the `shows` table
			* the `id` is being used and interpreted as `show_id` in `genres` table
			* but `id` is officially defined primarily in `shows` table

Now you may see why these file are called [[CS50x 7-3 SQL#Relational database|relational database]]
* we can create / define the relation between tables
* tables are linked together by a common column
	* or some common columns
	* typically the common columns store some numbers or ids
* in our example
	* the table `shows` basically just store the `title` column, but assign to the row an `id`
	* the table `genres` however is not like any column in the original data
		* `genres` looks like this
```sqlite
sqlite> SELECT * FROM genres;
+---------+-----------+
| show_id |   genre   |
+---------+-----------+
| 1       | Comedy    |
| 2       | Comedy    |
| 2       | Crime     |
| 2       | Drama     |
| 2       | Horror    |
| 3       | Drama     |
| 3       | Family    |
| 3       | Sport     |
| 4       | Animation |
...
```
* to create a table like this requires some logic that SQL is not very good at, that's why we would need a Python program to do that
* but this table is a better design than [[#Limitation of CSV|before]]
	* even though we double the number of table from 1 to 2
	* why it's better? because it's cleaner.
		* `'genre'` of show number 2 used to be `"Comedy, Crime, Drama, Horror"`
		* now each genre of show number 2 is categorized as, has its own cell
		* so show number 2 is associated with multiple cells in `genre` column
			* it's called a *one-to-many relationship*

Now perhaps it's easier to search every TV shows that is a `"Comedy"`
maybe we can just search in the `genres` table?
```sqlite
sqlite> SELECT show_id FROM genres WHERE genre = "Comedy";
```
but this off course just gives us the `show_id` but not the `title`
to solve this, things start to get crazy again

we can actually compose one SQL question from multiple ones
```sqlite
sqlite> SELECT title FROM shows WHERE id IN (SELECT show_id FROM genres WHERE genre = "Comedy");
```
* to improve this a bit maybe we can also do `ORDER BY title`
* maybe also do `DISTINCT(title)` to filter out duplicates

> [!quote]
> I don't just start at the beginning and type out my whole thought, and just get it right on the first try. I very commonly start with the subquery, the thing in parentheses `()`, just to get myself one step toward what I care about. Then I add to it. Then I add to it. Then I add to it.

___

# `INSERT`

say if we want to `INSERT` the show `"Seinfeld"` to the tables
```sqlite
-- we don't need to care about the id, so skip it --
sqlite> INSERT INTO shows (title) VALUES ("Seinfeld");
```

but we also have to flag it as at least one genre
```sqlite
-- before doing so, we would need to find its id, how? --
sqlite> SELECT * FROM shows WHERE title = "Seinfeld";
+-----+----------+
| id  |  title   |
+-----+----------+
| 159 | Seinfeld |
+-----+----------+
-- then we can flag its genre(s), by inserting it to the table 'genres' --
sqlite> INSERT INTO genres (show_id, genre) VALUES (159, "Comedy");
```

the last thing is
actually all the other show's `'title'` are capitalized, so let's capitalize this new show `'title'` as well
```sqlite
UPDATE shows SET title = "SEINFELD" WHERE title = "Seinfeld";
```
___

# Real world use cases

Well, thus far we've been playing around data with SQL and its CRUD operations pretty manually.
But we can essentially automate all of these by writing code in Python that generates SQL operations

eg. **Log-in**
if we go to most any website on the Internet today, let say Google, and log in
we would enter username and password right? and then click Sign In.
What happen after?

Although the website might be implemented not in Python, but certainly in some language, Python, JavaScript, Java, Ruby, etc.
There is probably some code that will
1. use SQL to get our username and password from a relational database
2. compare what we typed in with the username and password it got from the database
	* actually hopefully it's not getting the actual password
	* but getting something called a *hash*

eg. **Buy something on Amazon**
When we click Check Out
1. There is some code looking what are added to our shopping cart
2. Some SQL `INSERT` commands are perform to store what we brought in Amazon's database

Although in real world not every company or application use SQL databases or relational databases, maybe they use some other types of database,
relational databases are quite popular
___

# Python and SQL

now let's look at how we can sort of merge Python and SQL together
* we're gonna use SQL inside of a Python program
* it's like our logic is in Python but we would use SQL to talk to the database

```python
# --------
# this is a program to search
# the total number of people who like a particular show
# --------

# cs50's SQL is rather simple but easy to use
from cs50 import SQL

# open up a database, in the current directory
db = SQL("sqlite:///favorites.db")

title = input("Title: ").strip()

# `db.execute()` executes a SQL command, and returns a list of row
#    also each row is a dictionary
# `?` in `db.execute()` in Python, is like `%s` in `printf()` in C
rows = db.execute("SELECT COUNT(*) FROM favorites WHERE title LIKE ?", title)

# print the cell in row 1, column "COUNT(*)"
# we have only 1 row in this table, because of how the way `COUNT` works
row = rows[0]
print(row["COUNT(*)"])
```
actually we can give the output column a nickname
```python
...
rows = db.execute("SELECT COUNT(*) AS counter FROM favorites WHERE title LIKE ?", title)

row = rows[0]
print(row["counter"])
```

why would we do something like this?
* in Python code we may make benefit from searching or manipulating the database
* but we don't have to deal with the `with open()` something something, and the `DictReader()` or `reader()`, and then do a `for` loop, etc.
	* instead it would be just one line of SQL command
* sort of using the best of both worlds
___

# eg. IMDb
[InternetMovieDatabase.com](https://www.imdb.com/?ref_=nv_home)
it's a website where we can search for TV shows, movies, actors, and so forth, all using their database behind the scenes

The database is available as not CSV file, but *TSV file*
* TSV: tab-separated values
* folks in CS50 downloaded the TSV files and imported them into a database `shows.db`
	* through a Python program similar to the [[#^77a6be|favorites8.py]] example

now we can use `sqlite3 shows.db` to open up the database
it maybe a good idea to enter `sqlite> .schema` to learn about the database's structure

```sqlite
sqlite> .schema
CREATE TABLE shows (
                    id INTEGER,
                    title TEXT NOT NULL,
                    year NUMERIC,
                    episodes INTEGER,
                    PRIMARY KEY(id)
                );
CREATE TABLE genres (
                    show_id INTEGER NOT NULL,
                    genre TEXT NOT NULL,
                    FOREIGN KEY(show_id) REFERENCES shows(id)
                );
CREATE TABLE stars (
                show_id INTEGER NOT NULL,
                person_id INTEGER NOT NULL,
                FOREIGN KEY(show_id) REFERENCES shows(id),
                FOREIGN KEY(person_id) REFERENCES people(id)
            );
CREATE TABLE writers (
                show_id INTEGER NOT NULL,
                person_id INTEGER NOT NULL,
                FOREIGN KEY(show_id) REFERENCES shows(id),
                FOREIGN KEY(person_id) REFERENCES people(id)
            );
CREATE TABLE ratings (
                show_id INTEGER NOT NULL,
                rating REAL NOT NULL,
                votes INTEGER NOT NULL,
                FOREIGN KEY(show_id) REFERENCES shows(id)
            );
CREATE TABLE people (
                id INTEGER,
                name TEXT NOT NULL,
                birth NUMERIC,
                PRIMARY KEY(id)
            );
```

there are some clever uses of tables to take away
* `shows` and `genres` are related in a way just like [[#Table Relationships|ours]]
* `stars` table doesn't mention the shows or people
	* but `show_id` and `person_id` instead
* both `stars` and `writers` reference `id` from the table `people`
	* every person in the IMDb database universe is in the `people` table and is given an `id`

also the `people` table alone can have `13,058,202` pieces of data already
so it's not going to open very well in Excel / Google Sheets / Apple Numbers
SQL may be the only choice...
___
