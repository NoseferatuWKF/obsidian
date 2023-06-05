## psql

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

## SQL

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
> by default it should be ASC first
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
> virtual table that is queryable
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
> - `INNER JOIN`: Returns records that have matching values in both tables
> - `LEFT JOIN`: Returns all records from the left table, and the matched records from the right table
> - `RIGHT JOIN`: Returns all records from the right table, and the matched records from the left table
> - `CROSS JOIN`: Returns all records from both tables

```sql
SELECT SUM(CITY.POPULATION) FROM CITY LEFT JOIN COUNTRY ON CITY.COUNTRYCODE = COUNTRY.CODE WHERE COUNTRY.CONTINENT = 'Asia';

-- with grouping
SELECT COUNTRY.CONTINENT, FLOOR(AVG(CITY.POPULATION))
FROM COUNTRY
CROSS JOIN CITY
ON COUNTRY.CODE = CITY.COUNTRYCODE
GROUP BY COUNTRY.CONTINENT;
```