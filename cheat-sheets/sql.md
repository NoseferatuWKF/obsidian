## Postgres

connect to db
```bash
psql postgresql://[user]:[password]@[host]:[port][...][/dbname]?[param=value&...]
```

commands
```bash
\l # list dbs
\c # connect to db
\d # list tables
\q # quit
\x on # expanded display use \x auto for both table and expanded
\watch [SEC] # execute query every SEC seconds
\! clear # clear term
```

get number of connections
```SQL
SELECT sum(numbackends) FROM pg_stat_database;
```

MySQL UPSERT like using ON CONFLICT
```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...)
ON CONFLICT (conflict_column)
DO NOTHING | DO UPDATE SET column1 = value1, column2 = value2, ...;

-- example
INSERT INTO inventory (id, name, price, quantity)
VALUES (1, 'A', 16.99, 120) -- where id 1 already exists
ON CONFLICT(id) 
DO UPDATE SET
  -- update the required columns
  price = EXCLUDED.price,
  quantity = EXCLUDED.quantity;
```

## MSSQL

repeat query
```SQL
insert into table (column1, column2)
values ('abc', 123)

go 100 -- n times of repeat
```

# SQL

>double quotes for column names, single quotes for values

SELECT
```SQL
-- select with multiple conditionals
-- case will return twice
SELECT DISTINCT CITY FROM STATION WHERE 
CITY LIKE 'a%' OR
CITY LIKE 'e%' OR
CITY LIKE 'i%' OR
CITY LIKE 'o%' OR
CITY LIKE 'u%';

-- regex
SELECT DISTINCT CITY FROM STATION WHERE CITY RLIKE '^[aeiouAEIOU].*[aeiouAEIOU]$'
-- regex with NOT
SELECT DISTINCT CITY FROM STATION WHERE CITY NOT RLIKE '^[aeiouAEIOU]';

-- advanced
-- concat can also use parentheses and || to combine strings
SELECT CONCAT(NAME, '(', LEFT(OCCUPATION, 1), ')') FROM OCCUPATIONS ORDER BY NAME;

-- lower must be combined with group by
SELECT CONCAT('There are a total of ', COUNT(OCCUPATION), ' ', LOWER(OCCUPATION), 's.') FROM OCCUPATIONS GROUP BY OCCUPATION ORDER BY COUNT(OCCUPATION), OCCUPATION;

-- postgres casting
SELECT CONCAT(columnA, LEFT(CAST(id as varchar), 6)) FROM table;
```

AGGREGATION
```SQL
-- ceil rounding and replace 0 with nothing
SELECT CEIL(AVG(SALARY) - AVG(REPLACE(SALARY, '0', ''))) FROM EMPLOYEES;
```

ORDER BY
>by default it should be ASC first
```SQL
-- double query with ordering and limit
SELECT CITY,LENGTH(CITY) FROM STATION ORDER BY LENGTH(CITY) ASC, CITY LIMIT 1;
SELECT CITY,LENGTH(CITY) FROM STATION ORDER BY LENGTH(CITY) DESC, CITY LIMIT 1;

-- sort starting from the right of varchar
SELECT NAME FROM STUDENTS WHERE MARKS > 75 ORDER BY RIGHT(NAME, 3), ID ASC;
```

UPDATE
```SQL
UPDATE table
SET column = 'value'
WHERE column like '%value%'

-- with coalesce to select first non null value
UPDATE table
SET column = coalesce(column2, column2)
WHERE column ~ 'regex'

-- replace regex with cast and concat
UPDATE users
SET identifier = CONCAT(REGEXP_REPLACE(identifier, '_deleted_[0-9]', '_deleted_'),
LEFT(CAST(id AS varchar), 6))
WHERE identifier ~ '_deleted_[0-9]'

-- set column based on another column value filtered by base condition
UPDATE table
SET column =
	CASE WHEN another_column='condition' then some_other_column
	ELSE default_column
	END
WHERE identifier = 'another_condition'
```

ALTER
```SQL
-- drop column
ALTER table table_name
DROP COLUMN col

-- add column with default value
ALTER table table_name
ADD COLUMN col smallint NOT NULL DEFAULT 0

-- rename column
ALTER table table_name
RENAME COLUMN column_name TO new_column_name
```

INSERT INTO
```SQL
INSERT INTO table (column1, column2)
VALUES ('a', 'b')
```

TRUNCATE()
```SQL
TRUNCATE <table> CASCADE -- cascade to remove all fk constraints

-- truncate to 4 decimal places
SELECT TRUNCATE(LAT_N, 4) FROM STATION WHERE LAT_N < 137.2345 ORDER BY LAT_N DESC LIMIT 1;
```

COUNT()
```SQL
SELECT COUNT(*) -- or column name
FROM table
-- calculate diff between unique fields from all fields
SELECT COUNT(field) - COUNT(DISTINCT field)
FROM table
```

CREATE VIEW
>virtual table that is queryable
```SQL
CREATE VIEW pq AS (
    SELECT 
        CASE WHEN occupation = 'Doctor' THEN name END AS 'Doctor',
        CASE WHEN occupation = 'Professor' THEN name END AS 'Professor',
        CASE WHEN occupation = 'Singer' THEN name END AS  'Singer',
        CASE WHEN occupation = 'Actor' THEN name END AS  'Actor',
        ROW_NUMBER() OVER (PARTITION BY occupation ORDER BY name) as cr
    FROM occupations
);

SELECT MAX(Doctor),MAX(Professor),MAX(Singer),MAX(Actor) FROM pq 
GROUP BY cr
```

JOIN
>- `INNER JOIN`: Returns records that have matching values in both tables
>- `LEFT JOIN`: Returns all records from the left table, and the matched records from the right table
>- `RIGHT JOIN`: Returns all records from the right table, and the matched records from the left table
>- `CROSS JOIN`: Returns all records from both tables

```sql
SELECT SUM(CITY.POPULATION) FROM CITY LEFT JOIN COUNTRY ON CITY.COUNTRYCODE = COUNTRY.CODE WHERE COUNTRY.CONTINENT = 'Asia';

-- with grouping
SELECT COUNTRY.CONTINENT, FLOOR(AVG(CITY.POPULATION))
FROM COUNTRY
CROSS JOIN CITY
ON COUNTRY.CODE = CITY.COUNTRYCODE
GROUP BY COUNTRY.CONTINENT;
```

UNION
>combining results of two or more select statements
```sql
-- UNION selects distinct value by default
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
-- UNION ALL allows duplicates
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```