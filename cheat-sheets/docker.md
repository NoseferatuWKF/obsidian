>prefer containerd over docker

house keeping
```bash
docker kill $(docker ps -q) # unlike stop this fucks containers pronto
docker system prune # full clean except volumes
```

view
```bash
docker ps --size # check running container size
```

docker run
```bash
# run multiple commands within docker
docker run --rm busybox bash -c "whoami && pwd"

# runtime options, good for benchmarking
docker run --rm --cpu 0.8 --memory 256m busybox

# overwrite entrypoint, sadly can't do anything for CMD
docker run --entrypoint bash grafana/k6
```

cache busting
```Dockerfile
ARG CACHE_BUST=1
```

docker build, save and load somewhere else
>can be used to load images in cri for kubernetes when loading images
```bash
# build image
docker build -t image:latest .
# save image to local
docker save image:latest -o image.tar
# load image in another docker runtime
docker load -i image.tar
```

## more cheat-sheets:
- https://dockercheatsheet.com/