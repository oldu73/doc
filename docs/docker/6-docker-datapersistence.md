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

### Development environment

Set up of a development environnement for a node server application (c.f. [Node server project](5-docker-nodeserverimage.md#node-server-project)).  

The goal here is to have changes available from host machine (IDE) in container and automatically reloaded by nodemon.

!!Warning!! A common mistake is to bind the root folder of a project.

E.g. in case of node context, if we do so, the node_modules folder created/populated in container image by "RUN npm install" instruction from Dockerfile will be erased. To avoid this unwanted behavior, it's mostly advised to move the source code files modified through IDE on host machine in a dedicated folder (e.g. src).

On host machine:
```
$ mkdir node-server
$ cd node-server
$ mkdir src
$ cd src
$ touch app.js
$ cd ..
```

src/app.js:
```
const express = require('express');

const app = express();

app.get('*', (req, res) => {
    res.status(200).json('Hello, world!');
})

app.listen(80);
```

Dockerfile (in root folder, "node-server"):
```
FROM node:alpine
WORKDIR /app
COPY ./package.json .
RUN npm install
COPY . .
ENV PATH=$PATH:/app/node_modules/.bin
CMD ["nodemon", "-L", "src/app.js"]
```

!! Warning !! In above Dockerfile we use **-L** option for --legacy-watch because in some containerized environnement, application may not restart automatically after file's change.

Build image:
```
$ docker build -t myapp .
```

Start a container with new image version:
```
$ docker run --rm -p 80:80 --mount type=bind,source="$(pwd)/src",target=/app/src myapp
```

Now try to edit 'src/app.js' file on host machine to observe nodemon restart in terminal.  
You may also observe the changes on your browser at 'localhost' address, after refresh.
