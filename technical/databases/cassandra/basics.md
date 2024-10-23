# Case Studies

[Build microservices using AWS Keyspaces and AWS ECS](https://aws.amazon.com/blogs/database/build-microservices-using-amazon-keyspaces-and-amazon-elastic-container-service/)
[Cassandra vs. ScyllaDB: Evolutionary Differences](https://www.youtube.com/watch?v=AewGSnGxjwk)

# Caveats

- need to know query ahead of time as rows are stored across multiple nodes, and to optimize the query it should served by a single node
- immutable data stores, meaning data does not get updated in place, instead the data is pointed to a new data
- data is not deleted immediately, tombstone is used to mark obsolete data and removed on first compaction
- eventual consistency, meaning data that gets queried might not be the latest

## Read ahead

| Hardware       | Recommendation |
| -------------- | -------------- |
| Spinning Disks | 64KB           |
| SSD            | 4KB            |

# Architecture 

## Overview

design objectives
- full multi-master database replication
- global availability at low latency
- scaling out on commodity hardware
- linear throughput increase with each additional processor
- online load balancing and cluster growth
- partitioned key-oriented queries
- flexible schema

out of scope
- cross partition transactions
- distributed joins
- foreign keys or referential integrity

concepts
- keyspace: contains tables, defines how a dataset is replicated
- table / column family: contains rows and columns, partitioned based on the partition key
- partition key: combination of primary key(s) and possible clustering keys
- primary key: precedes clustering key, determines which node will store the data
- clustering key: determine physical order of data within a partition
- row: collection of columns identified by a primary key
- column: a single datum with a type

CQL advanced features
- single partition lightweight transactions with atomic compare and set semantics
- user-defined types, tuples, functions and aggregates
- storage-attached indexing (SAI) for secondary indexes
- local secondary indexes (2i)
- collection types including sets, maps, and lists
- local secondary indices

# Dynamo

## Partitioning

techniques
- consistent hashing to reduce rebalancing on node additions
- last write wins to resolve conflicting mutations
- token ring similar to chord algorithm for data redundancy on replica
- vnodes to evenly space out the token ring by adding a virtual node corresponding to a physical node

## Virtual Nodes(vnodes)

nomenclature
- token
- endpoint
- host Id
- vnode
- token map (tokens to endpoints)

| pros                         | cons                                                     |
| ---------------------------- | -------------------------------------------------------- |
| equal distribution of data   | the more the tokens, the higher probability of an outage |
| even query load distribution | slower cluster-wide maintenance<br>                      |
|                              | performance over a span token range could be affected    |

## Replication Strategy

| NetworkTopologyStrategy                     | SimpleStrategy                                              |
| ------------------------------------------- | ----------------------------------------------------------- |
| individual RF for each datacenter           | single integer RF                                           |
| specify replicas within a rack using Snitch | treats all nodes identically, ignoring datacenters or racks |

## Data Versioning
- mutation timestamp on every column of every row within a CQL partition
- either from client clock or, absent a client provided time-stamp, from the coordinator node's clock
- replica read repair for read synchronization, hinted handoff for write synchronization
- anti-entropy repair using Merkle trees for full replica synchronization
- sub-range repair and incremental repair to increase the resolution of the hash trees

## Tunable Consistency

CL
- ONE: only a single replica must respond
- TWO: two replicas must respond
- THREE: three replicas must respond
- QUORUM: a majority (n/2 + 1) of replicas must respond
- ALL: all replicas must respond
- LOCAL_QUORUM: a majority of replicas in the local datacenter must respond
- EACH_QUORUM: a majority of replicas in each datacenter must respond
- LOCAL_ONE: only a single replica in the local datacenter must respond
- ANY: a single replica may respond, or the coordinator may store a hint
- write operations are always sent to all replicas regardless of CL

## Distributed Cluster Membership and Failure Detection

gossip
- updates local node's heartbeat state
- picks random other node to exchange gossip state
- probably attempts to gossip with any unreachable nodes
- gossips with a seed node if step 2 did not happen

seed node
- can be bootstrap into the ring without seeing any other seed nodes
- hotspots for gossip
- non-seed nodes must be able to contact at least one seed node to join the cluster

failure detection
- Phi Accrual Failure Detector
- convicted nodes will stop getting reads
- a node will never be removed without explicit instruction

# Storage Engine

Write path
- Logging data in the commit log
- writing data to the memtable
- flushing data from the memtable
- storing data on disk in the SSTables

Read path
- read from memtable
- check row cache, if enabled
- checks bloom filter
- checks partition key cache, if enabled
- goes directly to the compression offset map if a partition key is found in the partition key cache, or checks the partition summary if the partition summary is checked, then the partition index is accessed
- locates the data on disk using the compression offset map
- fetches data from SSTable

Commit log
- append only logs of all mutations local to node
- any data written will be written to commit log first before written to memtable
- separated by commitlog_segment_size

Memtables
- in-memory structure
- flushed to SSTables when exceeding threshold of commit-log approaches max size

SSTables
- Data.db
- Index.db
- Summary.db
- Filter.db
- CompressionInfo.db
- Statistics.db
- Digest.crc32
- TOC.txt

# Configuring

[cassandra.yaml](https://cassandra.apache.org/doc/latest/cassandra/managing/configuration/cass_yaml_file.html)
```yaml
cluster_name: # Default Test Cluster. Name of cluster
num_tokens: # Default 256. Proportion of data relative to other nodes, initial_token will override this
initial_token: # Default blank. When vnodes are not used or there is only one token per node, the intial token specifies where in the range this node belongs. If blank (default), Cassandra will try to determine which nodes in the cluster have the largest load, and it will bisect the range of those nodes. If Cassandra cannot determine which nodes have the highest load, it will select a random token. 
authenticator: # Default org.apache.cassandra.auth.AllowAllAuthenticator. Java class used for authentication with Cassandra
authorizer: # Default org.apache.cassandra.auth.AllowAllAuthorizer. Java class used for limiting and granting permissions to Cassandra objects
permissions_validity_in_ms: # Default 2000. When using Authorizer, this specifies how long to cache permissions
partitioner: # Default org.apache.cassandra.dht.Murmur3Partitioner. Java class used for partitioning the data among the nodes
data_file_directories: # Default /var/lib/cassandra/data. Data storage location
commitlog_directory: # Default /var/lib/cassandra/commitlog. commit log location
disk_failure_policy: # Default stop. Cassandra will stop Gossip and Thrift on the node. if best_effort is used, Cassandra will attempt to use the remaining good disks
saved_caches_directory: # Default /var/lib/cassandra/saved_caches. Saved caches location
commitlog_sync: # Default periodic. The rate of fsync between commit log and disk
commitlog_sync_period_in_ms: # Default 10000. The time which fsync writes to disk
commitlog_segment_size_in_mb: # Default 32. Size of commit log per file
seed_provider: # Default org.apache.cassandra.locator.SimpleSeedProvider. Java class used to provide the seeds that will allow nodes to autodetect the cluster
concurrent_reads: # Default 32. Number of concurrent reads allowed
concurrent_writes: # Default 32. Number of concurrent writes allowed
memtable_total_space_in_mb: # Default not specified. If specified, Cassandra will flush the largest MemTable when the limit has been reached. If not specified, Cassandra will flush the largest MemTable when it reaches 1/3 of the heap
listen_address: # Default localhost. The address the host listens on
```

# Compaction

## Types

- Minor compaction
- Major compaction
- User defined compaction
- Scrub
- UpgradeSSTables
- Cleanup
- Secondary index rebuild
- Anticompaction
- Sub range compaction

## Strategies

- Unified Compaction Strategy (UCS)
- Size Tiered Compaction Strategy (STCS)
- Leveled Compaction Strategy (LCS)
- Time Window Compaction Strategy (TWCS)

# Logs

## Common log files

- system.log
- debug.log
- gc.log