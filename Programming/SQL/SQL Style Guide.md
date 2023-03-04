[SQL style guide by Simon Holywell](https://www.sqlstyle.guide/#general)
___

# Do

* Consistent and descriptive identifiers and names
* Good use of white space and indentation

* Use [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) to store time and date information
	* `YYYY-MM-DD HH:MM:SS.SSSSS`

* Try to use standard SQL functions instead of vendor-specific functions
	* for portability

* Keep code succinct. No redundant SQL.
	* such as unnecessary quoting `''` or parentheses `()`
	* or `WHERE` clauses that can otherwise be drived

* Include comments in SQL code where necessary
```sql
/* Descrip about the query */
UPDATE file_system
   SET file_modified_date = '1980-02-22 13:19:01.00000',
       file_size = 209732  -- explain this line of code
 WHERE file_name = '.vimrc';
```
___

# Avoid

* CamelCase
	* difficult to scan quickly
* Descriptive prefixes or Hungarian notation
	* like `sp_` or `tbl`
* Plurals
	* use a more natural collective term instead, where possible
	* use `staff` instead of `employees`.
	* use `people` instead of `individuals`.

* Quoted identifiers
	* if you must use them then stick to SQL-92 double quotes for portability

* Object-oriented design principles should not be applied to SQL or database structures
___

# Naming conventions


**General**

* Ensure the name is unique
* Careful not to name something that exist as a [reserved keyword](https://www.sqlstyle.guide/#reserved-keyword-reference)
* Keep the length of a name to a maximum of 30 bytes
	* = 30 characters unless we're some how using a multi-byte character set

* Begin with a letter. Not end with an underscore `_`
* Only use letters, numbers and underscores `_`
* Avoid using multiple consecutive underscores `__`
	* hard to read
* Use underscores `_` where we would naturally include a space
	* eg. name 'first name' as `first_name`

* Avoid *abbreviations* ( 縮寫 )


**Tables**

* Use a collective name, or less ideally, a plural form.
	* `staff`, or less ideally, `employees`

* Do not prefix with `tbl`, or any other descriptive prefix or Hungarian notation
* Never give a table the same name as one of its columns. vice versa.
* Avoid concatenating two table names together to create the name of a relationship table
	* eg. avoid `cars_mechanics`. use `services`


**Columns**

* Use a singular name.
* Do not add a column with the same name as its table. vice versa.
* Always use lowercase
	* except where it may make sense not to, such as proper nouns

* Avoid simply using `id` as the primary identifier for the table.
___

# Query syntax


**Reserved words**

* Always use uppercase for the [reserved keyword](https://www.sqlstyle.guide/#reserved-keyword-reference)
	* like `SELECT` and `WHERE`
* Avoid the abbreviated keywords. Use the full length ones.
	* prefer `ABSOLUTE` to `ABS`

* Do not use database server specific keywords, if an ANSI SQL keyword already exists performing the same function
	* make the code more portable


**White space**

* Use to line up the code
	* form a river down the middle, separating the keywords from the implementation detail
```sql
(SELECT f.species_name,
        AVG(f.height) AS average_height, AVG(f.diameter) AS average_diameter)
   FROM flora AS f
  WHERE f.species_name = 'Banksia'
     OR f.species_name = 'Sheoak'
     OR f.species_name = 'Wattle'
  GROUP BY f.species_name, f.observation_date)

  UNION ALL

(SELECT b.species_name,
        AVG(b.height) AS average_height, AVG(b.diameter) AS average_diameter
   FROM botanic_garden_flora AS b
  WHERE b.species_name = 'Banksia'
     OR b.species_name = 'Sheoak'
     OR b.species_name = 'Wattle'
  GROUP BY b.species_name, b.observation_date);
```

* Include spaces
	* before and after equals `=`
	* after commas `,`
	* surrounding apostrophes `'`
		* where not within parentheses `()`
		* or with a trailing comma `,` or semicolon `;`
```sql
SELECT a.title, a.release_date, a.recording_date
  FROM albums AS a
 WHERE a.title = 'Charcoal Lane'
    OR a.title = 'The New Danger';
```


**Line spacing**

* Include newlines / vertical space
	* before `AND` or `OR`
	* after semicolons `;`
		* to separate queries
	* after each keyword definition
	* after a comma `,` when separating multiple columns into logical groups
	* to separate code into realted sections
		* helps reading

```sql
INSERT INTO albums (title, release_date, recording_date)
VALUES ('Charcoal Lane', '1990-01-01 01:01:01.00000', '1990-01-01 01:01:01.00000')
       ('The New Danger', '2008-01-01 01:01:01.00000', '1990-01-01 01:01:01.00000');
```

```sql
UPDATE albums
   SET release_date = '1990-01-01 01:01:01.00000'
 WHERE title = 'The New Danger';
```

```sql
SELECT a.title,
       a.release_date, a.recording_date, a.production_date -- grouped dates together
  FROM albums AS a
 WHERE a.title = 'Charcoal Lane'
    OR a.title = 'The New Danger';
```


**Indentation**

* `JOIN` should be indented to the other side of the river and grouped with a new line, if necessary
```sql
SELECT r.last_name
  FROM riders AS r
	   INNER JOIN bikes AS b
	   ON r.bike_vin_num = b.vin_num
	      AND b.engine_tally > 2
	
	   INNER JOIN crew AS c
	   ON r.crew_chief_last_name = c.last_name
	      AND c.chief = 'Y';
```

* Subqueries should be indented to the right side of the river, then laid our using the same style as any other query
	* sometimes it will make sense to have the closing parenthesis `)` on a new line at the same character position as its opening partner `(`
	* especially true where we have nested subqueries
```sql
SELECT r.last_name,
	   (SELECT MAX(YEAR(championship_date))
	      FROM champions AS c
	     WHERE c.last_name = r.last_name
	       AND c.confirmed = 'Y') AS last_championship_year
  FROM riders AS r
 WHERE r.last_name IN
	   (SELECT c.last_name
	      FROM champions AS c
	     WHERE YEAR(championship_date) > '2008'
	       AND c.confirmed = 'Y');
```


**Preferred formalisms**

* Make use of `BETWEEN` instead of combining multiple statements with `AND`
* Similarly use `IN()` instead of multiple `OR` clauses
* Make use of [`CASE` statement](https://www.w3schools.com/sql/sql_ref_case.asp) if possible
	* can be nested to form more complex logical structures

* Avoid the use of `UNION` clauses and temporary tables where possible
	* if the schema can be optimised to remove the reliance on these features then it most likely should be

```sql
SELECT CASE postcode
	   WHEN 'BN1' THEN 'Brighton'
	   WHEN 'EH1' THEN 'Edinburgh'
	   END AS city
  FROM office_locations
 WHERE country = 'United Kingdom'
   AND opening_time BETWEEN 8 AND 9
   AND postcode IN ('EH1', 'BN1', 'NN1', 'KW1');
```
___
