# `INDEX`

if we perform seaching or any other operation on a database as large as the [[CS50x 7-4 Table Relationships#eg. IMDb|IMDb database]], it's going to be slow, no doubt

let say we want to perform a small search, searching for all the shows with the title `"The Office"`
we enter
```sqlite
sqlite>.timer on -- actually SQL has a built in timer to time the operation
sqlite> SELECT * FROM shows WHERE title = "The Office";
```
it would take some milliseconds, in our example 0.021 seconds

how we can speed up the operation, is by creating `INDEX`
```sqlite
CREATE INDEX "title_index" ON "shows" ("title");
```

now if we run `SELECT * FROM shows WHERE title = "The Office";` again
it would take 0.000 seconds

what happen?
* the first search, the slow one, is doing [[CS50x 3-2 Linear and Binary Search#Linear Search|Linear Search]]
* but as we create an index `"title_index"` on the `"title"` column
	* it would do something to the table to speed things up
	* maybe it's using a [[CS50x 5-3 Tree|tree]], [[CS50x 5-5 tries|trie]], [[CS50x 5-4 Hash Table|table]], or some fancy two-dimensional data structure
	* so it's much faster to find something
		* than finding things in one long list
* but recall there is a [[CS50x 5-2 Linked Lists#^917ee6|trade off]]
	* as we create the indexes, it takes up some more space in memory
___

# B-trees
in fact the type of structure that's often used in a database to store data, is a thing called B-tree
it does not stand for binary

![[B-tree.png]]
* it's a rather wide, but shallow, not very tall, tree
* the database can search pretty quickly in this kind of tree
* it's going to take some time to build up this structure though
	* some time is spended up front
	* but then thereafter every subsequent query is going to be faster
___

# Search across tables
how might the indexes help further?

let's for instance find all of `"Steve Carell"`'s TV shows, still using the [[CS50x 7-4 Table Relationships#eg. IMDb|IMDb example]]
* unfortunately `shows` and `people` are 2 separated tables, which have no relation to each other
* but there is a *join table* `stars`, connecting / joining the 2 tables logically
* let's go ahead and do the search step-by-step

1. we want to find `"Steve Carell"` from the `people` table
```sqlite
sqlite> SELECT * FROM people WHERE name = "Steve Carell";
+--------+--------------+-------+
|   id   |     name     | birth |
+--------+--------------+-------+
| 136797 | Steve Carell | 1962  |
+--------+--------------+-------+
-- actually we only care about the id --
sqlite> SELECT id FROM people WHERE name = "Steve Carell";
+--------+
|   id   |
+--------+
| 136797 |
+--------+
```

2. using `"Steve Carell"`'s `id`, we can find all the shows he was involved from the `stars` table
```sqlite
sqlite> SELECT show_id FROM stars WHERE person_id = (SELECT id FROM people WHERE name = "Steve Carell");
```

3. it gives us the `show_id`, but not the shows' `title`, so let's fix it
```sqlite
sqlite> SELECT title FROM shows WHERE id IN (SELECT show_id FROM stars WHERE person_id = (SELECT id FROM people WHERE name = "Steve Carell"));
```

4. maybe also sort the result
```sqlite
sqlite> SELECT title FROM shows WHERE id IN (SELECT show_id FROM stars WHERE person_id = (SELECT id FROM people WHERE name = "Steve Carell")) ORDER BY title;
```

although the search is quite long
* it gets across 3 tables after all
* the search time is still rather short
	* in our example it's 0.094 seconds

but let's try creating some more indexes
```sqlite
-- create indexes on each column that is somehow involved in our search query --
sqlite> CREATE INDEX person_index ON stars (person_id);
sqlite> CREATE INDEX show_index ON stars (show_id);
sqlite> CREATE INDEX name_index ON people (name);
-- then let's try run the big query again --
sqlite> SELECT title FROM shows WHERE id IN (SELECT show_id FROM stars WHERE person_id = (SELECT id FROM people WHERE name = "Steve Carell")) ORDER BY title;
```
* it'd spend almost no time
	* in our example it's 0.001 second

if anytime we use some applications and wondering why is the thing so slow?
* it could be it's working with a large database which has thousands of rows
* and it's not properly indexed
	* so think about what column should be optimized for searches and filtration when writing our own applications
* still, it could be just bad algorithm and bad code though
___

# `JOIN`

one more thing,
our ultimate search command is quite long isn't it?
```sqlite
sqlite> SELECT title FROM shows WHERE id IN (SELECT show_id FROM stars WHERE person_id = (SELECT id FROM people WHERE name = "Steve Carell")) ORDER BY title;
```

what we can do to make it look perhaps a bit better is using a `JOIN` operator
```sqlite
sqlite> SELECT title FROM people JOIN stars ON people.id = stars.person_id JOIN shows ON stars.show_id = shows.id WHERE name = "Steve Carell" ORDER BY title;
```

okay how to read this command?
1. `people JOIN stars`
	* somehow join or combine the tables `people` and `stars`
2. `ON people.id = stars.person_id`
	* here is how to join `people` and `stars`
	* we line up `stars.person_id` with `people.id`
	* so it's like the we originally have the `people` table, then we combine the `stars` table to the `people` table, but in a way that each row refers to the same person's `id`
3. `JOIN shows ON stars.show_id = shows.id`
	* further combine the `shows` table to the `people + stars` table
	* this time we line up `shows.id` with `stars.show_id`
4. `WHERE name = "Steve Carell"`
5. `SELECT title`
6. `ORDER BY title`

lastly this doesn't make use of `JOIN` but still give the same result
```sqlite
sqlite> SELECT title FROM people, stars, shows
   ...> WHERE people.id = stars.person_id
   ...> AND stars.show_id = shows.id
   ...> AND name = "Steve Carell";
```
* we can hit Enter to create a new line
	* but it's actually is a new command
* if we go back through the history by hitting Up Arrow key one time
	* it'll go to `   ...> AND name = "Steve Carell";`
	* we'd have to go back through the history multiple times to perform the same operation

* performance-wise there are no difference
* `JOIN` is arguably better because we don't need the `WHERE` clause
___
