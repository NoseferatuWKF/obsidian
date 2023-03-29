## psql

connect to db within network
```bash
psql postgresql://[user[:password]@][host][:port][,...][/dbname][?param1=value1&...]
```

psql commands
```bash
\c # connect to db
\q # quit
```

## SQL

UPDATE
```SQL
UPDATE table
SET column = 'value'
WHERE column like '%value%'
```