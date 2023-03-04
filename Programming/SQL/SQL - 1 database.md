# What is database?

Sure enough, a database is a thing to **store information**.
* eg. username and password combination
	* if the combination match a data in database, let the user log in
* eg. a user's shopping history

A database is much like Microsoft Excel or Google Spreadsheets or others
* within a database, we have tables
	* within a table, we have
		* rows and columns
* eg. let's pretend a database is a Microsoft Excel file
	* in a file, `Book1`, it has 2 sheets
		* `Sheet1` and `Sheet2`
		* *sheet* in the world of database, would be *table*
	* each sheet has columns and rows
		* together they point to a *cell*
___

# SQL
*Structured Query Language*

* it's a programming language
* its sole purpose in life, is to **query** a database
	* = ask questions of or retrieve data from a database
	* therefore it has 4 types of command only -- [[CS50x 7-3 SQL#CRUD|CRUD]]


There are many different implementations of SQL. Among them, 2 rather popular ones are
* MySQL
	* an open-source platform
	* commonly used to establish [[CS50x 7-3 SQL#Relational database|relational database]]
* SQLite
	* used in CS50 since 2016
	* has a very similar feature set to MySQL, but more lightweight
	* a little easier to use on CS50 IDE

**phpMyAdmin**
* it's a GUI tool to execute database queries
* many installations of SQL come with it
* although we still would prefer executing commands programmatically
	* because we want things to happen automatically for us
	* we don't want to log in some program, clicking buttons, to do something
		* we don't want to manually do anything at all
	* we write code or program that does that for us
* similarly we also have **phpLiteAdmin**

fortunately SQL integrates really nicely with other programming languages such as [[CS50x 7-4 Table Relationships#Python and SQL|Python]] or PHP
* they have functions that we can use to connect to, query, and make changes to our databases
___

# Create database

Creating a database is just as simple as creating a file
Creating a table though, the syntax is actually a bit awkward to do programmatically
That's where phpMyAdmin will come in handy

Also, to create a table, the program first is going to ask us which columns are gonna be stored in that table
* eg. the columns may be user id, username, password
* we have to design ahead of time what the table is going to look like

Thereafter, all the queriers will refer to **rows** of the table
* except deleting the database or the table
___

# Data Types in SQL

Each column of a table is capable of holding data of a particular data type
* In C we have `char`, `int`, `float`, etc.
* In SQL we have way more types
	* Here is 20 of them

```sql
INT, SMALLINT, TINYINT, MEDIUMINT, BIGINT
DECIMAL, FLOAT
BIT
DATE, TIME, DATETIME, TIMESTAMP
GEOMETRY, LINESTRING
TEXT
ENUM
CHAR, VARCHAR
BINARY, BLOB
```

`INT`
* there are different `INT` types
* each has different [[C - 1 data types#int|upper bounds]]

`DECIMAL`
* = `double` in C
`FLOAT`
* = `float` in C

`DATE, TIME, DATETIME, TIMESTAMP`

`GEOMETRY` and `LINESTRING`
* can be used to mapping out or drawing out an area on a map
	* maybe using a *GIS system*

`TEXT`
* = strings

`ENUM`
* actually this exist in C too
* an enum is a limited set of values
	* eg. we can specify an enum `my_favorite_color` to has only, or be capable of holding only, `red`, `green`, `blue`, 3 values
	* therefore all the rows in that column in the table would have values either be `red`, `green` or `blue` only
		* if we try to insert a row with `purple` in that place, that wouldn't work

`CHAR`
* very different from C, `CHAR` in SQL is not a single character
* it's more like a fixed-length string
	* also when we specify a `CHAR` or `VARCHAR`, we have to specify the length of that string
	* much like how [[CS50x 4-2 string and address#string and char|string]] works in C
* if the column type is `CHAR(10)`
	* we can store 10-characters strings in that column
	* but exactly 10 characters
		* if we try storing `"HI!"`, it would be okay
			* but there would also be 7 extra characters, which are the equivalent of [[CS50x 2-3 Array#NUL|NUL]]
		* how about storing a 15-characters word?
			* we end up storing the first 10 letters
`VARCHAR`
* refers to a *variable-length string*
* we specify the maximum possible length of a string
	* eg. `VARCHAR(99)`
* if we store `"HI!"`, this time however would not have any extra null character tacked onto the end

also see [suggested column data types](https://www.sqlstyle.guide/#column-data-types) for maximum compatibility
___

# Data Types in SQLite

SQLite has many or all of the these same data types as well
but the 20 some data types are *affiliated* as *"type affinity"* to simplify things
* it means data types that are alike, are considered as or reduced to the same type
* `NULL`, `INTEGER`, `REAL`, `TEXT`, `BLOB`

`NULL`
* = nothing

`INTEGER`
* = whole number

`REAL`
* includes things like decimal and float

`TEXT`
* includes things like `CHAR` and `VARCHAR`, and more

`BLOB`
* basically it's any data that would have large number of bits or bytes
	* but not `TEXT`
___

# Primary Key
* it's a column from the table
* we need to choose a column to be the primary key when we create a table

Why do we need it?
* Every row of our table, should be able to be uniquely and quickly identified
	* to make our SQL queries most effective
* For that, values or every row in one of the columns should be unique
	* then we can very quickly identify which row we're talking about

In addition it's possible to establish a *joint primary key*
* it's a combination of two columns that is always guaranteed to be unique
* eg. we have a table containing values `A, B, C, D, ...` and another table containing values `1, 2, 3, 4, ...`
	* then when the 2 columns are jointed, values are going to be `A1, A2, A3, ... , B1, B2, ...`
	* the values are still unique, so it's acceptable
___
