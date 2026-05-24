# SQL-Log

SQL-Query-Log
Here's a log of my SQL skillset!

Aggregate Functions
AVG() - returns an average value
ROUND() to specify precision after a decimal
COUNT() - returns number of values
MAX() - returns maximum value
MIN() - returns minimum value
SUM() - returns the sum of all values

ALTER Table
Allows for changes to an existing table structure

Adding columns
ALTER TABLE table_name
ADD COLUMN new_col TYPE

Alter constraints
ALTER TABLE table_name
ALTER COLUMN col_name
SET DEFAULT value

(Can also ADD CONSTRAINT constraint_name in place of SET DEFAULT value)

Example Query:
ALTER TABLE information
RENAME TO new_info

ALTER TABLE new_info
RENAME COLUMN person TO people

ALTER TABLE new_info
ALTER COLUMN people DROP NOT NULL

BETWEEN
Used to match a value against a range of values.

SELECT COUNT column
FROM table
WHERE column BETWEEN 8 AND 9

Can input NOT BETWEEN to return values not inbetween the designated values.

CASE
Executes an SQL code when certain conditions are met, (similar to an IF/ELSE statement)

General CASE Example query:
SELECT customer_id,
CASE
WHEN (customer_id <= 100) THEN 'Premium' WHEN (customer_id BETWEEN 100 AND 200) THEN 'Plus'
ELSE 'Normal'
END AS customer_class
FROM customer

Another example:
SELECT
SUM(CASE rental_rate
WHEN 0.99 THEN 1
ELSE 0
END) AS number_of_bargains
FROM film

This would return a summed number of everything given the value 1
CAST
Converts one data type to another

Must be reasonable conversion
Ex: '5' to an integer will work, 'five' to an integer will not
Syntax:
SELECT CAST('5' AS INTEGER)

Can use in a SELECT query with a column name instead of a single instance.
Example:
SELECT CAST(date AS TIMESTAMP)
FROM table

Changing the case of a string
Upper Case
SELECT UPPER(email)
FROM customer

Lower Case
SELECT LOWER(email)
FROM customer

Title Case
SELECT INITCAP(title)
FROM film

CHECK
Creates more customized constraints that adhere to a certain condition.
Example: Making sure all inserted integer values fall below a certain threshold.

Example query:
CREATE TABLE employees(
emp_id SERIAL PRIMARY KEY,
first_name VARCHAR(50) NOT NULL,
last_name VARCHAR(50) NOT NULL,
birthdate DATE CHECK (birthdate > '1900-01-01'),
hire_date DATE CHECK (hire_date > birthdate),
salary INTEGER CHECK (salary > 0)
)

COALESCE
Accepts an ulimited number of arguments. It returns the first argument that is not null. If all arguments are null, the COALESCE function will return null.

Useful when querying a table that contains null values and substituting it with another value.
Example query:
SELECT item,(price - COALESCE(discount,0))
AS final FROM table

Example query:
SELECT COALESCE(country, 'Both countries') AS country,
COALESCE(medal, 'All medals') AS medal,
COUNT(* ) AS awards
FROM summer_medals
WHERE year = 2008 AND COUNTRY IN ('CHN, 'RUS')
GROUP BY ROLLUP(country, medal)
ORDER BY country ASC, medal ASC);

COUNT/COUNT DISTINCT
The COUNT function returns the number of input rows that match a specific condtion of a query.
COUNT DISTINCT will return only the distinct number of values from a column.

SELECT COUNT (name) FROM table

SELECT COUNT(DISTINCT name)
FROM table

CREATE TABLE
General syntax:

CREATE TABLE players(
player_id SERIAL PRIMARY KEY,
age INTEGER NOT NULL,
)

Another example:
CREATE TABLE account(
user_id SERIAL PRIMARY KEY,
username VARCHAR(50) UNIQUE NOT NULL,
password VARCHAR(50) NOT NULL,
email VARCHAR(250) UNIQUE NOT NULL,
created_on TIMESTAMP NOT NULL,
last_login TIMESTAMP
)

SERIAL - typical for the primary key type as it logs unique integer entries automatically upon insertion

DELETE
Removes rows from a table

Syntax:
DELETE FROM table
WHERE row_id = 1

Delete rows based on their presence in other tables
Example:
DELETE FROM tableA
USING tableB
WHERE tableA.id=tableB.id

Delete all rows from a table
DELETE FROM table

DROP
Removes a column in a table along with its indexes and constraints.
Does not remove columns used in views, triggers, or stored procedures without the CASCADE clause.

General syntax:
ALTER TABLE table_name
DROP COLUMN col_name

(Insert CASCADE at the very end to remove all dependencies)

Check for existence to avoid error:
ALTER TABLE table_name
DROP COLUMN IF EXISTS col_name

(Can drop multiple columns as well)

FRAMES
ROWS BETWEEN
ROWS BETWEEN [START] AND [FINISH]

n PRECEDING: n rows before the current row

CURRENT ROW: the current row

n FOLLOWING: n rows after the current row

Examples:
ROWS BETWEEN 3 PRECEDING AND CURRENT ROW
ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
ROWS BETWEEN 5 PRECEDING AND 1 PRECEDING

RANGE BETWEEN:
RANGE BETWEEN [START] AND [FINISH]
Similar to ROWS BETWEEN
RANGE treats duplicates in OVER's ORDER BY subclause as a single entity

GROUP BY
Aggregates columns per category.

Example syntax:
SELECT customer_id,SUM(amount)
FROM payment
GROUP BY customer_id
ORDER BY SUM(amount)

HAVING
Places a filter after an aggregation has already taken place

SELECT company,SUM(sales)
FROM finance_table
WHERE company != 'Google'
GROUP BY company
HAVING SUM(sales) > 1000

IN
Creates a condition that checks to see if a value is included in a list of multiple options.

SELECT color
FROM table WHERE color IN ('red','blue')

INSERT
Add rows into a table
General syntax:
INSERT INTO table (column1, column2)
VALUES
(value1, value2),
(value1, value2)

Syntax for sharing values from another table:
INSERT INTO table(column1,column2)
SELECT column1,column2
FROM another_table
WHERE condition;

Note: SERIAL columns do not need to be provided a value

JOINS (INNER/OUTER/LEFT/RIGHT) | AS Statement | UNION
JOINS combine information from multiple tables

AS creates an "alias" for a column or result
Example syntax:
SELECT SUM(column) AS new_name
FROM table

INNER JOIN returns results that match two different tables
Examples syntax:
SELECT * FROM TableA
INNER JOIN TableB
ON TableA.col_match = TableB.col_match

FULL OUTER JOIN Combines unique values from two different tables
Example syntax:
SELECT * FROM customer
FULL OUTER JOIN payment
ON customer.customer_id = payment.customer_id
WHERE customer.customer_id IS null
OR payment.payment_id IS null

LEFT OUTER JOIN Returns set of records that are in the left table AND in both tables. If there is no match with the right table, the results are null
(Order does matter!)

RIGHT OUTER JOIN The same as LEFT OUTER JOIN but reversed

UNION Combines the results of tow or more SELECT statements.
Essentially concatenates two results together.

Example syntax:
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2

LIKE/ILIKE
Performs pattern matching against string data with the use of wildcard characters

% - matches any sequence of characters
_ - matches any single character

LIKE is case-sensitive
ILIKE is not

Example syntax:
SELECT * FROM customer
WHERE first_name ILIKE 'J%' AND last_name LIKE '%her%'

LIMIT
Limits the number of rows returned for a query.
Goes at the very end of a query

SELECT column
FROM table
ORDER BY column DESC
LIMIT 5

Logical Operators
Combine multiple comparison operators

AND
OR
NOT

NULLIF
Takes 2 inputs and returns NULL if both are equal. Otherwise it returns the first argument passed.

Useful in cases where a NULL value would cause an error or unwanted result
Example:
NULLIF(10,10)

Returns NULL
NULLIF(10,12)

Returns 10
ORDER BY
Sorts rows based on a column value, in ascending or descending order.

SELECT column_1, column_2
FROM table
ORDER BY column_1 ASC/DESC

REPLACE
Replace characters in a string.

SELECT REPLACE(description, 'A Astounding', 'An Astounding') AS description
FROM film

REVERSE
Reverses the order of a string.

SELECT title, REVERSE(title)
FROM film AS f

ROLLUP and CUBE
ROLLUP GROUP BY subclause that includes extra rows for group-level aggregations.

Example syntax:
SELECT country, medal, COUNT(*) AS awards
FROM summer_medals
WHERE year = 2008 AND country IN ('CHN', 'RUS')
GROUP BY country, ROLLUP(country, medal)
ORDER BY country ASC, medal ASC;

GROUP BY COUNTRY, ROLLUP(medal) will count all country- and medal- level totals, then count only country- level totals and fill in medal with nulls for these rows.
ROLLUP is hierarchical, de-aggregating from the leftmost provided column to the right-most
ROLLUP(country, medal) includes country- level totals<br/? ROLLUP(medal, country) includes medal-level totals

CUBE
CUBE is a non-hierarchical ROLLUP
It generates all possible group-level aggregations

CUBE(country,medal) counts country-level, medal-level, and grand totals

SELF-JOIN
A query in which a table is joined to itself.
Useful for comparing values in a column of rows within the same table.
It is necessary to use an alias for the table.

Example syntax:
SELECT tableA.col, tableB.col
FROM table AS tableA
JOIN table AS tableB ON
tableA.some_col = tableB.other_col

SELECT f1.title, f2.title, f1.length
FROM film AS f1
INNER JOIN film AS f2 ON
f1.film_id = f2.film_id
AND f1.length = f2.length

String Functions and Operators
Edits, combines and alters text data columns

Example syntax:
SELECT LENGTH(first_name) FROM customer
Example return: 5

SELECT title, CHAR_LENGTH(title)
FROM film;

SELECT first_name || last_name FROM customer
Example return: JackJohnson

SELECT first_name || ' ' || last_name
Example return: Jack Johnson

SELECT LOWER(LEFT(first_name,1)) || LOWER(last_name) || '@gmail.com'
FROM customer
Example return: jjohnson@gmail.com

SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM customer;

POSITION
SELECT email, POSITION('@' IN email)
FROM customer;

STRPOS Analogous to POSITION SELECT email, STRPOS(email, '@')
FROM customer;

LEFT
SELECT LEFT(description, '50') FROM film;

RIGHT
SELECT RIGHT(description, '50') FROM film;

SUBSTRING
Extract a substring from data.

SELECT SUBSTRING(description, 10, 50) FROM film AS f;
Starting at the 10th character, the next 50 characters will be returned.

SELECT SUBSTRING(email FROM 0 FOR POSITION('@' IN email))
FROM customer;
Example return: MARY.SMITH (returns everything before '@' in the email)

Example Query: Extracting only the street name from an address.
SELECT SUBSTRING(address FROM POSITION(' ' IN address)+1 FOR LENGTH(address))
FROM address;

TRIM
Removes characters from the start, middle or end of a string.
TRIM([leading | trailing | both] [characters] FROM string)

SELECT TRIM(' padded ');
Removes all whitespace from the beginning and end of a string.

LTRIM or RTRIM
Removes only the spaces at the beginning or end of a string, not both.
SELECT LTRIM(' padded ');

SELECT RTRIM(' padded ');

Example:
SELECT
RPAD(first_name, LENGTH(first_name)+1)
|| RPAD(last_name, LENGTH(last_name)+2, ' <')
|| RPAD(email, LENGTH(email)+1, '>') AS full_email
FROM customer;

LPAD
SELECT LPAD('padded', 10, '#');
Returns: ####padded
Note: Returns a character length of 10 and fills blanks space with the # symbol.

STRING_AGG
STRING_AGG(column, separator) takes all the values of a column and concatenates them, with separator in between each value

SubQuery
Performs a query on the results of another query

SELECT title,rental_rate
FROM film
WHERE rental_rate >
(SELECT AVG(rental_rate) FROM film)

SELECT film_id,title
FROM film
(SELECT inventory.film_id
FROM rental
INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id
WHERE return_date BETWEEN '2005-05-29' AND '2005-05-30')

SELECT first_name,last_name
FROM customer AS c
WHERE EXISTS
(SELECT * FROM payment AS p
WHERE p.customer_id = c.customer_id
AND amount > 11)

Text Search
to_tsvector and tsquery
SELECT to_tsvector(description)
FROM film;

SELECT title, description
FROM film
WHERE to_tsvector(title) @@ to_tsquery('elf');

TIMESTAMPS and EXTRACT
TIME - Contains only time
DATE - Contains only date
TIMESTAMP - Contains date and time
TIMESTAMPTZ - Contains date, time, and timezone

TIMEZONE
NOW
TIMEOFDAY
CURRENT_TIME
CURRENT_DATE

EXTRACT() - Obtain a sub-component of a date value (year, month, day, week, or quarter)
Example syntax:
EXTRACT(YEAR FROM date_col)

SELECT EXTRACT(MONTH FROM payment_date)
AS pay_month
FROM payment

AGE() - Calculates and returns the current age given to a timestamp
AGE(date_col)
Example return: 13 years 1 mon 5 days 01:34:13.003423

TO_CHAR() - Converts data types to text
Usage:
T0_CHAR(date_col,'mm-dd-yyyy')

SELECT TO_CHAR(payment_date,'MONTH-YYYY')
FROM payment

UPDATE
Changes values of columns in a table

General syntax:
UPDATE table
SET column1 = value1,
column2 = value2
WHERE
condition;

Example:
UPDATE account
SET last_login = CURRENT_TIMESTAMP
WHERE last_login IS NULL;

UPDATE join
UPDATE TableA
SET original_col = TableB.new_col
FROM tableB
WHERE tableA.id = TableB.id

To simply return affected rows, use the RETURNING function
Example:
UPDATE account
SET last_login = created_on
RETURNING account_id,last_login

User Defined Data Types
ENUM
Enumerated data types are great options to use in your database when you have a column where you want to store a fixed list of values that rarely change. Examples include the directions on a compass (i.e., north, south, east and west).

CREATE TYPE compass_position AS ENUM (
'north',
'south',
'east',
'west');

Query this as:
SELECT *
FROM pg_type
WHERE typname='compass_position';

VIEW
Stores a specific query

Example query:
CREATE VIEW customer_info AS
SELECT first_name,last_name,address FROM customer
INNER JOIN address
ON customer.address_id = address.address_id

When you want to use this VIEW, you would simply input:
SELECT * FROM customer_info

To modify the VIEW, input CREATE OR REPLACE VIEW at the beginning of the query and then make the needed adjustments to the VIEW syntax

WHERE
The WHERE function specifies conditions on columns for the rows to be returned.

SELECT column1, column2
FROM table
WHERE conditions (ex: name='David')

Window Functions
FIRST_VALUE(column)
Returns the first value in the table or partition.

LAST_VALUE(column)
Returns the last value in the table or partition.

Example Syntax for FIRST_VALUE and LAST_VALUE:
SELECT year, city
FIRST_VALUE(city) OVER(ORDER BY year ASC) AS first_city,
LAST_VALUE(city) OVER(ORDER BY year ASC
RANGE BETWEEN
UNBOUNDED PRECEEDING AND
UNBOUNDED FOLLOWING)
AS last_city
FROM hosts
ORDER BY year ASC;

ROW_NUMBER()

SELECT col_name,
ROW_NUMBER() OVER() AS alias
FROM table_name
ORDER BY alias;

LAG
Returns column's value at the row n rows before the current row.

SELECT column1, column2
LAG(column2, 1) OVER (ORDER BY column1 ASC)
FROM table
ORDER BY column1 ASC;

LEAD
Returns column's value at the row n rows after the current row.

WITH hosts AS (
SELECT DISTINCT year, city
FROM summer_medals)

SELECT year, city,
LEAD(city, 1) OVER (ORDER BY year ASC) AS next_city
LEAD(city, 2) OVER (ORDER BY year ASC) AS after_next_city
FROM hosts
ORDER BY year ASC;

NTILE(n)
Splites the data into n approximately equal pages.

WITH disciplines AS (
SELECT DISTINCT discipline
FROM summer_medals)

SELECT discipline, NTILE(15) OVER () AS page
FROM disciplines
ORDER BY page ASC;

WITH Athlete_Medals AS (
SELECT Athlete, COUNT() AS Medals
FROM Summer_Medals
GROUP BY Athlete
HAVING COUNT() > 1)

SELECT
Athlete,<b/r> Medals,
NTILE(3) OVER() ORDERY BY medals AS Third
FROM Athlete_Medals
ORDER BY Medals DESC, Athlete ASC;

PARTITION BY
Splits the table into partitions based on a column's unique values

SELECT column1, column2, column3,
LAG(column3) OVER (PARTITION BY column2
ORDER BY column2 ASC, column1 ASC)
FROM table_name
ORDER BY column2 ASC, column1, ASC

ROW_NUMBER()
Assigns unique numbers to rows, regardless of duplicates.

RANK()
Assigns the same number to rows with identical values, skipping over the next numbers in such cases.

DENSE_RANK()
Also assigns the same number to rows with indetical values, but doesn't skip over the next numbers.

Example syntax for ROW_NUMBER, RANK and DENSE_RANK:
SELECT country, games,
ROW_NUMBER() OVER (ORDER BY games DESC) AS row_n,
RANK() OVER (ORDER BY games DESC) AS rank_n,
DENSE_RANK() OVER (ORDER BY games DESC) AS dense_rank_n
FROM table
ORDER BY games DESC, country ASC;
