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
select sum(numbackends) from pg_stat_database;
```

copy to/from
```sql
copy table to /path/to/file.csv delimiter ',' csv quote '"';
copy table from /path/to/file.csv delimiter ',' csv quote '"';
```
