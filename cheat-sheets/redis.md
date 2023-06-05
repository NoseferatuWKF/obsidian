connect to remote
```bash
redis-cli -h <host|ip> -p <port>
```

monitoring
```bash
redis-cli monitor
```

basic operations
```bash
FLUSHALL # remove everything
keys <pattern> # list keys by pattern
get <key> # get values by key
del <key> # remove key(s)
```

advanced operations
```bash
# remove all keys matching pattern 
redis-cli keys "*some-key*" | xargs redis-cli del
```