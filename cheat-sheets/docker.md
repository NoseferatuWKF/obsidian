kill all running containers:
```bash
docker kill $(docker ps -q)
```

start [[networking/docker|docker]] on mac (will open the application not just run the service): 
```bash
open -a Docker
```

remove all unused containers, networks, images (both dangling and unreferenced)
```bash
docker system prune
```

### more cheat-sheets:
- https://dockercheatsheet.com/