# Four operations

SQL is a programming language with fairly limited vocabulary
since it's just handling [[CS50x 7-3 SQL#CRUD|CRUD]]

Therefore there are only 4 main operations that we'll most likely perform on tables
1. `INSERT`
2. `SELECT`
3. `UPDATE`
4. `DELETE`

also see [SQL Keywords Reference (w3schools.com)](https://www.w3schools.com/sql/sql_ref_keywords.asp)
and [SQL Tutorial (w3schools.com)](https://www.w3schools.com/sql/default.asp)
___

# eg. `users` and `moms`

We'll use these 2 tables, `users` and `moms`, as our example
for explaining the operations

`users`
| idnum | username | password | fullname |
| --- | --- | --- | --- |
| 10 | jerry | fus!\|\|! | Jerry Seinfeld |
| 11 | gcostanza | b0sc0 | George Costanza |

`moms`
| username | mother |
| --- | --- |
| jerry | Helen Seinfeld |
| gcostanza | Estelle Costanza |
___

# `INSERT`
* Add information to a table

* the structure of a `INSERT` command is
```sqlite
INSERT INTO
<table>     -- the table we want to insert information into
(<columns>) -- a comma-separated list of the columns that we want to insert data into
VALUES
(<values>)  -- a comma-separated list of the values that we want to put into <columns>

--- eg. ---
INSERT INTO
users
(username, password, fullname)
VALUES
('newman', 'USMAIL', 'Newman')
```

then what is the `users` table going to look like?
`users`
| idnum | username | password | fullname |
| --- | --- | --- | --- |
| 10 | jerry | fus!\|\|! | Jerry Seinfeld |
| 11 | gcostanza | b0sc0 | George Costanza |
| 12 | newman | USMAIL | Newman |

Wait a minute, we didn't specify `idnum`. Why is `12` there?
* column `idnum` is the table's primary key
	* we haven't told you that but we defined that column as the primary key beforehand
* also `idnum` column type is `INTEGER`
	* it's usually good idea to have the primary key be type `INTEGER`
	* it's not required, but usually is a good idea
* since it's type `INTEGER`, we can configure the column to **auto-increment**
	* so even if we forget to specify a value for the column
	* it will automatically insert a value there, when an new row is added to the table
		* that value is unique from every other value in that column
		* typically it's just incrementing by one

Okay, what happen if it's not auto-increment, and we forgot to populate it?
* There might be a couple of rows that are blank or null
* Then it would not be unique any more
___

# `SELECT`
* Extract information from a table

* it's structure is
```sqlite
SELECT
<columns>
FROM
<table>
-- optional --
WHERE
<condition / predicate> -- predicate is a more proper word in the context of SQL
ORDER BY
<column>

--- eg. ---
SELECT
idnum, fullname
FROM
users
```

what will happen is
* it would give to us the `users` table
* but only show the column `idnum` and `fullname`
`users`
| idnum | fullname |
| --- | --- |
| 10 | Jerry Seinfeld |
| 11 | George Costanza |
| 12 | Newman |

now say if we want to restrict the search a little bit
```sqlite
SELECT
password
FROM
users
WHERE
idnum < 12
```

the it'd show to us?
* the `users` table
* but 1 column only, `password`
* and row `fus!||!` and `b0sc0` only

one more example
```sqlite
SELECT
*      -- '*' is a short-hand for every column
FROM
moms
WHERE
username = 'jerry'
```

result is
`moms`
| username | mother |
| --- | --- |
| jerry | Helen Seinfeld |
___

# `UPDATE`
* Modify information in a table

The skeleton looks like this
```sqlite
UPDATE
<table>
SET
<column> = <value>
WHERE
<predicate>

--- eg. ---
UPDATE
users
SET
password = 'yadayada'
WHERE
idnum = 10
```

it's pretty straight-forward. it just do the following change.
`users`
| idnum | username | password | fullname |
| --- | --- | --- | --- |
| 10 | jerry | fus!\|\|! --> yadayada | Jerry Seinfeld |
| 11 | gcostanza | b0sc0 | George Costanza |
| 12 | newman | USMAIL | Newman |
___

# `DELETE`
* Remove information from a table

```sqlite
DELETE FROM
<table>
WHERE
<predicate>

--- eg. ---
DELETE FROM
users
WHERE
username = 'newman'
```

after we execute this command, the row whose `username` is `'newman'`, got deleted
`users`
| idnum | username | password | fullname |
| --- | --- | --- | --- |
| 10 | jerry | fus!\|\|! | Jerry Seinfeld |
| 11 | gcostanza | b0sc0 | George Costanza |
___

# `cat <file> | sqlite3 <database>`

aside from running queries in the program `sqlite3`
we also can run queries in the VS Code terminal, by running  `cat <file> | sqlite3 <database>`

eg.
`cat filename.sql | sqlite3 songs.db`
where `filename.sql` is the file containing the SQL query
___
