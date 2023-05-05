kill all running containers:
```bash
docker kill $(docker ps -q) # unlike stop this fucks containers pronto
```

start [[networking/docker|docker]] on mac (will open the application not just run the service): 
>or just use finch or orbstack for GUI
```bash
open -a Docker
```

remove all unused containers, networks, images (both dangling and unreferenced)
```bash
docker system prune
```

run multiple commands within docker
```bash
docker run --rm busybox bash -c "whoami && pwd"
```

cache busting
```Dockerfile
ARG CACHE_BUST=1
```

## more cheat-sheets:
- https://dockercheatsheet.com/