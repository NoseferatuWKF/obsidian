## psql

connect to db
```bash
psql postgresql://[user[:password]@][host][:port][,...][/dbname][?param1=value1&...]
```

commands
```bash
\l # list dbs
\c # connect to db
\d # list tables
\q # quit
\x on # expanded display use \x auto for both table and expanded
\watch [SEC] # execute query every SEC seconds
```

get number of connections
```SQL
SELECT sum(numbackends) FROM pg_stat_database;
```

## SQL

>double quotes for column names, single quotes for values

UPDATE
```SQL
UPDATE table
SET column = 'value'
WHERE column like '%value%'
```

INSERT INTO
```SQL
INSERT INTO table (column1, column2)
VALUES ('a', 'b')
```

TRUNCATE
```SQL
TRUNCATE <table> CASCADE -- cascade to remove all fk constraints
```