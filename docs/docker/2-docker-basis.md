# Docker - 02 - Basis

***

## Node

Command to launch node with app.js in folder app
CMD ["node", "app/app.js"]

Then from terminal, build command with -t argument to name:tag image
. to mention path of Dockerfile in current directory:

```
$ docker build -t node-test-001:latest .
```

To check:

```
$ docker images (or $ docker image ls)
```

To test:

```
$ docker run node-test-001
Hello, world!
```

To auto delete container after execution, use –rm option:

```
$ docker run --rm node-test-001
Hello, world!
```

Check if node is installed on host machine:

```
$ node --version
v14.16.1
```

Test app.js on host machine (if node is installed):

```
$ node app.js 
Hello, world!
```

***

## VS Code

VS Code install Microsoft Docker extension

***

## Docker file in short

My image -> Docker file =:

- Base image

- Modification

- Action

***

## Add Alpine package

Add/del Alpine package

A base image (e.g. Alpine) is not based on any other image.

Add or del package in Alpine

```
$c apk update

$c apk add grep

$c apk del grep
```

***

## Running container

Running container as a running process

To demonstrate running container is just a running process on host machine

```
$ docker run -d redis
```

In contrary of a virtual machine (VM) a container is "just" a running process
sharing Linux kernel on host machine.

SRC: - https://stackoverflow.com/questions/64787125/why-doesnt-htop-show-my-docker-processes-using-wsl2
To see running process on WSL use command prompt (would be "$ sudo ps -ef | grep redis" on a Linux machine):

```
C:\> wsl -d docker-desktop top
(or C:\> wsl -d docker-desktop ps -ef)
```

If you want htop, you need to install it first:

```
C:\> wsl -d docker-desktop apk update 
C:\> wsl -d docker-desktop apk add htop

... 0% redis-server *:6379
```

To kill a process on host machine, WSL:

```
C:\> wsl -d docker-desktop killall redis-server
```

To kill a process on host machine, Linux:

```
$ sudo killall redis-server
```

Running:

```
C:\> wsl -d docker-desktop htop
```

See that container isn't running anymore:

```
$ docker container ls
```

***

## Disk usage

Show docker disk usage

```
$ docker system df
```

Show detailed information on space usage, -v, --verbose

```
docker system df -v
```

***

## Consumed resource

to see live consuming resources of running containers:

```
$ docker container stats
```

***

## Inspect

to inspect all configuration of a container

```
$ docker container inspect alpinetest001
```

***

## Running process

Show running process in a container from host

```
$ docker container top alpinetest001
```

See the difference from inside the container

```
$ docker attach alpine001
```
1. update
```
$c apk update
```
2. add bash
```
$c apk add bash
```
3. test bash
```
$c bash
$c echo $0
bash
ctrl+p+q
```

```
$ docker exec -it alpinetest001 bash
```

if ps not present, install it with:

```
$c apk update && apk add procps
```

then:

```
$c ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 16:15 pts/0    00:00:00 /bin/sh
root        16     1  0 16:27 pts/0    00:00:00 bash
root        17     0  0 16:28 pts/1    00:00:00 bash
root        31    17  0 16:33 pts/1    00:00:00 ps -ef
```

sh and bash does not have parent process ID (PPID) = '0' because of container isolation,
container does not see running processes of host machine neither the ones belonging to other containers.

***

## Modified file

Show modified file in a container:

A = added, D = deleted, C = modified

```
$ docker container diff alpinetest001
A /test
A /test/test1.txt
A /test/test2.txt
C /root
A /root/.ash_history
```

***

## Copy file

copy file from host to container, docker cp path container:path

```
docker cp test1.txt alpinetest001:test
```

copy file from container to host, docker cp container:path path

```
docker cp alpinetest001:test/test2.txt .
```

***

## Execute command in container

Execute a command in a container without using terminal

```
$ docker container exec alpine001 mkdir testdir

$ docker container exec alpine001 touch /testdir/hello.txt
```

other e.g.

```
$ docker run -d --name redis001 redis

$ docker exec -it redis001 redis-cli

redis command:

set cle 42
get cle
exit
```

***

## Get shell in container

Get a shell, in a no matter which running container it is:

```
$ docker exec -it redis001 bash
$c echo $0
bash
```

if bash not installed in container (e.g. with alpine) you may use sh instead:
(if both presents, both works (e.g. redis)

```
$ docker exec -it redis001 sh
$c echo $0
sh
```

```
$ docker run -it -d --name alpine001 alpine

$ docker exec -it alpine001 bash
OCI runtime exec failed: exec failed: container_linux.go:380: starting container process caused: exec: "bash": executable file not found in $PATH: unknown

$ docker exec -it alpine001 sh
$c echo $0
sh
```

***

## Pause/unpause a container

```
$ docker container start -ai alpine001

$ docker container pause alpine001

$ docker container unpause alpine001
```

***

## Rename a container

rename a container named "beautiful_leakey"

```
$ docker container rename beautiful_leakey alpine001

not allowed to rename image
```

***

## Postgres with environnement variable

```
$ docker run -d --name mongo mongo
$ docker run -d --name redis redis
$ docker run -d --name postgres postgres

$ docker logs postgres

Error: ...

$ docker container rm postgres

$ docker container run --name postgres -d -e POSTGRES_HOST_AUTH_METHOD=trust postgres
```

***

## Stop container

Stop all running container at once

```
docker stop $(docker ps -aq)
```

***

## Suppress all

!Suppress all not used (stopped container(s) and not used for the rest)!

```
$ docker system prune -a
```

***

## Remove image

```
$ docker image rm NAME_OR_ID
```

remove unused images (dangling = image is not tagged and no other image depends on it)

```
$ docker image prune -a
```

***

## Remove container

try to remove a running container

Launch a background test named redis container with:

```
$ docker run --name test -d redis
```

then try to remove it with:

```
$ docker container rm test

Error - Stop the container before attempting removal or force remove
```

Force remove running container:

```
$ docker container rm -f test

$ docker run --name test1 -d redis
$ docker run --name test2 -d redis
$ docker run --name test3 -d redis

$ docker container rm -f test1 test2 test3
```

remove all stopped container

```
$ docker container prune
```

***

## Image, images

```
$ docker images

$ docker image ls redis
```

***

## Help

to get help, simply type:

```
$ docker
```

help on a command:

```
$ docker ps --help
```

***

## Redis

```
$ docker run redis

$ docker run -d redis
```

***

## Alpine

```
$ docker run alpine
```

-i, interactive mode

```
$ docker run -i alpine
$c ls

.
.
dev
etc
home
.
.
.

exit
```

-t, terminal -> prompt

```
$ docker run -it alpine (= docker run -it alpine sh)

$c echo $0	// check which shell (/bin/sh)
$c apk update
$c apk add bash
$c bash
$c echo $0	// check which shell (bash)
```

start in foreground mode

```
$ docker run alpine ping google.ch
$ docker run alpine echo hello
```

start in background mode (-d = detach (!= daemon))

```
$ docker run -d alpine ping google.fr
3593...
docker logs 3593...
$ docker logs 8e86... --follow
```

***

## Available image

show available image(s)

```
$ docker images
```

***

## None running container

check none running container with (-a show all containers (default shows just running)):
```
$ docker container ls -a
```

***

## Ubuntu

```
$ docker run -it ubuntu bash
```

```
$c cat /etc/os-release 

NAME="Ubuntu"
VERSION="20.04.3 LTS (Focal Fossa)"
...
```

to exit container and stop it

```
$c ctrl + d
```

then to start/stop it again, e.g container name is 'trusting_yalow'

```
$ docker start trusting_yalow

$ docker stop trusting_yalow
```

then to bash into it

```
$ docker attach trusting_yalow
```

to detach from a docker container without stopping it

```
$c ctrl + p + q
```

***

## Hello, world!

```
$ docker run hello-world
```

***

## Info

```
$ docker info
```

***
