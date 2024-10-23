accessing cqlsh on docker
```bash
docker exec -it <node> cqlsh
```

# [Nodetool](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/nodetool.html)

troubleshooting
```bash
nodetool status # check node status
nodetool info # list node info
nodetool statusbackup # check if incremental backup is enabled
nodetool proxyhistograms # list latency distributions
nodetool tablehistograms <keyspace> <table> # list local latency
nodetool tpstats # list threadpool state
nodetool compactionstats # check compaction state
nodetool datapaths # list directories that Cassandra uses
nodetool datapaths -- system_auth # list IAM directories
```

snapshots
```bash
# create snapshot for all tables in keyspace
nodetool snapshot --tag <tag-name> <keyspace>
# create snapshot for all tables in multiple keyspace
nodetool snapshot --tag <tag-name> <keyspace> <keyspace-2>
# create snapshot for a single table in keyspace
nodetool snapshot --tag <tag-name> --table <table-name> <keyspace>
# create snapshot for multiple tables in keyspace
nodetool snapshot --kt-list <keyspace>.<table>,<keyspace>.<table-2> --tag <tag-name>
# create snapshot for multiple tables in different keyspaces
nodetool snapshot --kt-list <keyspace>.<table>,<keyspace-2>.<table> --tag <tag-name>
# listing snapshots
nodetool listsnapshots
# delete snapshot by tag in keyspace
nodetool clearsnapshot -t <tag-name> <keyspace>
# delete all snapshots in keyspace
nodetool clearsnapshot --all <keyspace>
```

flush
>if incremental backup is enabled, flush will also trigger the incremental backup
```bash
# flush table in keyspace
nodetool flush <keyspace> <table>
```

bulk loading
```bash
# sstableloader
sstableloader --nodes <address> /path/to/keyspace/
# nodetool
nodetool import -- <keyspace> <table> /path/to/backup
```

# CQL

create keyspace
```cql
CREATE KEYSPACE keyspace_name WITH replication {'class': 'SimpleStrategy', 'replication_factor': 3};

USE keyspace_name;
```


create table
```CQL
CREATE TABLE table_name {
	id int,
	name text,
	PRIMARY KEY (id)
};
```

insert data
```cql
INSERT INTO table_name (id, name) VALUES (0, 'foo');
INSERT INTO table_name (id, name) VALUES (1, 'bar');
INSERT INTO table_name (id, name) VALUES (2, 'baz');
```
