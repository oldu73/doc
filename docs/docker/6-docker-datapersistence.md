# Docker - 06 - Data Persistence

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

Set up of a development environnement for a node server application (c.f. [Node server project](5-docker-nodeserver.md#node-server-project)).  

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

***

## Volumes

docker volume
- create
- inspect
- ls
- rm
- prune

old syntax (not advised, c.f. bind)
```
docker run -v <volume-name>:<container-url> image
```

new syntax
```
docker run --mount type=volume,source=<volume-name>,target=<url> image
```

### Create

#### New volume
```
$ docker volume create mydata
mydata

$ docker volume ls
DRIVER    VOLUME NAME
local     mydata

$ docker volume inspect mydata
[
    {
        "CreatedAt": "2021-12-24T06:15:34Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/mydata/_data",
        "Name": "mydata",
        "Options": {},
        "Scope": "local"
    }
]
```

WSL2 volumes are not in /var/lib/docker/volumes/mydata/_data

You can find WSL2 volumes under a hidden network share. Open Windows Explorer, and type **\\\wsl$** into the location bar. Hit enter, and it should display your WSL volumes, including the ones for Docker for Windows.

WSL2 volumes, in Windows Explorer bar
```
\\wsl$\docker-desktop-data\version-pack-data\community\docker\volumes
```

#### New container with bind volume
```console
$ docker run --mount type=volume,source=mydata,target=/data -it alpine sh
$c cd data
$c touch hello.txt
$c echo 123 > hello.txt
ctrl+d
$ docker rm containername
$ docker run --mount type=volume,source=mydata,target=/data -it alpine sh
$c cd data
$c cat hello.txt
123
```

### Share Backup Restore

#### Share volume between containers

```console
$ docker run --mount type=volume,source=mydata,target=/data --name firstcont -it alpine sh
$c1 cd data
In another terminal
$ docker run --mount type=volume,source=mydata,target=/data --name secondcont -it alpine sh
$c2 cd data
$c2 touch new.txt
$c1 ls
hello.txt  new.txt
```

Another way is volume from container
```console
$ docker run --volumes-from firstcont --name thirdcont -it alpine sh
```

#### Backup volume

Compress a volume to a bind folder with tar
```console
$ mkdir backup
$ docker run --mount type=volume,source=mydata,target=/data --mount type=bind,source="$(pwd)"/backup,target=/backup alpine tar -czf /backup/mydata.tar.gz /data
```

#### Restore volume
Extract an archive from a bind folder to a volume
```console
$ docker volume create restore

$ docker run --mount type=volume,source=restore,target=/data --mount type=bind,source="$(pwd)"/backup,target=/backup alpine tar -xf /backup/mydata.tar.gz -C /data
```

To not have a folder data in a folder data, use tar with option --strip-components 1
```console
$ docker run --mount type=volume,source=restore,target=/data --mount type=bind,source="$(pwd)",target=/backup -it alpine tar -xf /backup/backup.tar --strip-components 1 -C /data
```

Check volume correctly restored
```console
$ docker container run -it --rm --mount source=restore,target=/data alpine sh
```

### Volume to persist a database (mongo)
For mongo db it consist of mounting a volume with folder /data/db in mongo container
```console
$ docker volume create mydb
$ docker run --mount type=volume,source=mydb,target=/data/db -d --name mongocontainer1 mongo
$ docker exec -it mongocontainer1 sh
$c mongo
> use test
switched to db test
> db.user.insertOne({ name: 'jean' })
{
        "acknowledged" : true,
        "insertedId" : ObjectId("61caeae2df845609f1835264")
}
> db.user.findOne()
{ "_id" : ObjectId("61caeae2df845609f1835264"), "name" : "jean" }
> exit
bye
$c exit
$ docker container stop mongocontainer1
$ docker container rm mongocontainer1
```

Container is removed but database is persisted in a volume.
```console
$ docker run --mount type=volume,source=mydb,target=/data/db -d --name mongocontainer2 mongo
$ docker exec -it mongocontainer1 sh
$c mongo
> use test
switched to db test
> db.user.findOne()
{ "_id" : ObjectId("61caeae2df845609f1835264"), "name" : "jean" }
```

#### Compass GUI to browse mongo db

Should run container with opened port (default 27017).  
If port is already used on host machine, you may use 27018 for example

```console
$ docker run -p 27018:27017 --mount type=volume,source=mydb,target=/data/db -d --name mongocontainer3 mongo
```

Enter in connection field:  
mongodb://localhost:27018

***

## TMPFS

Rarely used, uniquely to keep data in RAM, e.g. secret or status data, works only on Linux system.
```console
$ docker run --mount type=tmpfs,target=/data -it alpine sh
$c cd data
$c touch secret.txt
$c ls
secret.txt
$c exit
$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS                     PORTS     NAMES
58f104d6a051   alpine    "sh"      2 minutes ago   Exited (0) 5 seconds ago             musing_wilbur
```

With 'TMPFS' if container is in an 'Exited' status, data aren't persisted.  
Relaunch container to observe that data aren't available anymore.
```console
$ docker start -ai musing_wilbur
$c cd data
$c ls
(empty)
```

***
