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
keys <pattern> # list key by pattern
get <key> # get values by key
```