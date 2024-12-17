# Topology

## Standalone in-memory cache
simplest, cache is in each instance of a program
## Distributed (client-server) cache
cache is in an external centralized server which a client can use a client library to access data over a network protocol 
## Replicated (in-process) cache
cache is in each instance of a program, and every change in the cache will be propagated to each instance of the same program
## Near-cache hybrid
local cache is in each instance of a program, and there is an external centralized cache that the program will use on cache miss

# Strategy

## Read-Through
on cache miss, data will be read from database instead
## Write-Through
on cache miss, data will be written on cache first, before written to database
## Write-Behind
on cache miss, data will be written on database, while asynchronously written to cache
## Write-Around
data is written directly to database, data read from database is written to cache
## Cache-Aside
no connection between cache and database, application controlled logic
## Pre-caching/Cache Priming
data is partially filled (warm cache) or fully filled (hot cache) on starting the program

# Common Pitfalls

## Stampede / Thundering Herd

## Saturation

## Stale Cache