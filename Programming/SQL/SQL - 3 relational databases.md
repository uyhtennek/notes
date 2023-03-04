# Relational database
see also [[CS50x 7-4 Table Relationships#Table Relationships|Table Relationships]]

okay. so using the [[SQL - 2 operations#eg. `users` and `moms`|same example]]
* if we want to also store a person's address, date of birth, social security number, etc.
* we would add new columns to the table `users`, right?

Well, we don't have to.
* if we do so the table would keep getting bigger and bigger until a point that it's annoying to use
* instead we can create another table to store yet other set of information
	* but we would have to define relationships between tables
		* that's why it's called *relational database*
	* otherwise we can't quite pull information across tables
___

# `SELECT (JOIN)`
* Extract information from multiple tables

let say we want to get
1. a user's fullname
	* stored in `users` table
2. with their mother's name
	* stored in `moms` table

How? as the [[SQL - 2 operations#`SELECT`|SELECT query]] we used before wouldn't work.
Here is how.
```sqlite
SELECT
<columns>
FROM
<table1>
JOIN
<table2>
ON
<predicate>

--- actually here is how ---
SELECT
users.fullname, moms.mother    -- pre-pending table names, just to be explicit
FROM
users
JOIN
moms
ON
users.username = moms.username -- pre-pending table names, because it's needed
```

1. `users JOIN moms`
* tables are joined, just temporarily, the 2 tables are not really merged forever
* it's just a temporary / hypothetical table that does merge the 2 tables

2. `ON users.username = moms.username`
* basically just finding where the 2 tables do overlap
	* like a row `FROM` `<table1>` should line up with which row `FROM` `<table2>`?

at this point, there is this temporary table
`users JOIN moms ON users.username = moms.username`
| users.idnum | users.username | users.password | users.fullname | moms.username | moms.mother |
| --- | --- | --- | --- | --- | --- |
| 10 | jerry | fus!\|\|! | Jerry Seinfeld | jerry | Helen Seinfeld |
| 11 | gcostanza | b0sc0 | George Costanza | gcostanza | Estelle Costanza |

3. `SELECT users.fullname, moms.mother`

therefore the result is
| users.fullname | moms.mother |
| --- | --- |
| Jerry Seinfeld | Helen Seinfeld |
| George Costanza | Estelle Costanza |
___
