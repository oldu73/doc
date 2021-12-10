# Docker - 6 - Data Persistence

***

## Introduction

Container =
- writable layer (this layer is deleted if the container no longer exists)
- image layer(s) (read)

Writable layers use UnionFS, slow read write performance, not adapted to host database.

To persist data, 3 possibilities:
### Volumes
On Filesystem but managed by Docker, accessible through Docker CLI), mostly advised to use (in /var/lib/docker/volumes/ (not on WSL) but never access it directly.
Volumes are stored on but isolated from host machine.

Create a volume with Docker CLI command
```
docker volume create
```

### Bind mount
Manged by Filesystem and accessible from outside of Docker, not recommended.
Bind mounts are regular folder and files stored on host machine.

### TMPFS
Temporary File System -> RAM.
TMPFS are used to store temporary not persisted data, sensitive information like secret.

***

## Bind mount

Adapted for:
- Sharing configuration files between host and container.
- Development environment to share source code and alow live reload.

Recommend syntax
```
docker run --mount type=bind,source=<url>,target=<url> image
```

```
$ mkdir data
$ cd data
$ touch hello.txt
$ cd ..
$ docker run --mount type=bind,source="$(pwd)"/data,target=/data -it alpine sh
$c ls
.. data ..
$c cd data
$c ls
hello.txt
```

Search for Mounts section with inspect CLI command to see mounting details:
```
docker container inspect containerName
```
