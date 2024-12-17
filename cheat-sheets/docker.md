login
```bash
docker login --username user host
```

house keeping
```bash
# kill all running containers 
docker kill $(docker ps -q)

# remove build cache
docker builder prune

# clean everything except volumes
docker system prune
```

pull and tag
```bash
docker pull <image>:<tag>
docker tag <image>:<tag> <new-image>:<new-tag>
```

run
```bash
# single file mount
docker run --mount type=bind,source=/local/file,target=/remote/file,readonly alpine

# ulimit
docker run --ulimit <type>=soft:hard # e.g: memlock=-1:-1

# run multiple commands within docker
docker run --rm busybox bash -c "whoami && pwd"

# runtime options, good for benchmarking
docker run --rm --cpu 0.8 --memory 256m busybox

# overwrite entrypoint, sadly can't do anything for CMD
docker run --entrypoint bash grafana/k6
```

cp
```bash
# container to local
docker cp container:/path/to/file/in/container /path/to/local

# local to container
docker cp /path/to/local container:/path/to/file/in/container
```

check every container running size
```bash
docker ps --size
```

build image from running container
```bash
docker commit container image:latest
docker run --rm -it image
docker push /path/to/registry
```

docker build, save and load somewhere else
>[!tip]
can be used to load images in cri for kubernetes when loading images
```bash
# build image
docker build -t image:latest .

# save image to local
docker save image:latest -o image.tar

# load image in another docker runtime
docker load -i image.tar
```

debug

init

compose watch