# Docker

***

$ = console

$c = console inside container

***

Project server node to return a minimal page in a browser at localhost address,
with Express (which is a Node.js framework)
  
Our image should have Node.js and its npm package manager.
It should have several dependencies: nodemon and express.
And it will have to launch the app contained in app.js by default.

For the project, we'll create a new Dockerfile in a folder.

In the same folder we will also have a package.json file (which allows you to manage the dependencies used) and an app.js file (which will contain our application).
In the package.json file therefore have our two dependencies.

In the app.js file we just have an Express route which will send in JSON format the character string "Hello, world!" for all routes.

folder: - /mnt/c/tmp/docker/node-server

$ touch package.json
edit package.json

{
    "dependencies": {
        "nodemon": "2.0.14",
        "express": "4.17.1"
    }
}

$ touch app.js
edit app.js

const express = require('express');

const app = express();

app.get('*', (req, res) => {
    res.status(200).json('Hello, world!');
})

app.listen(80);

$ touch Dockerfile

Browse a bit docker hub and look for alpine tagged image more lighter (169MB) than the official one (latest) (992MB)

FROM node:alpine

WORKDIR /app

COPY . .

RUN npm install

CMD ["nodemon", "app.js"]

then..

$ docker build -t myapp .

$ docker image ls

$ docker run --rm myapp

path error

to debug, run in interactive mode with sh to override Dockerfile CMD

$ docker run --rm -it myapp sh

/app # env

/app # nodemon
sh: nodemon: not found

# cd node_modules/.bin/
/app/node_modules/.bin # ls
is-ci      mime       nodemon

/app # ./node_modules/.bin/nodemon app.js

Fix the Docker file with adding value to image's path

..

ENV PATH=$PATH:/app/node_modules/.bin
..

$ docker build -t myapp .

$ docker run --rm -it myapp sh
/app # env | grep PATH
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/app/node_modules/.bin

/app # nodemon app.js

Now it's OK

$ docker run --rm myapp

Try "localhost" in a browser -> does not work and it's normal because default behavior does not allow
to communicate with a container.

###

for container
docker container export/import

$ docker build -t mynode .

$ docker run -it mynode sh

# touch hello.txt

# exit

$ docker container ps -a
CONTAINER ID   IMAGE
3d8e43e502b0   mynode

$ docker container export -o mycontainer.tar 3d8

$ tar -tvf mycontainer.tar

$ docker container rm 3d8

$ docker image import mycontainer.tar nodetest

$ docker images
REPOSITORY                                      TAG
nodetest                                        latest

$ docker image inspect nodetest:latest

Only one layer because exporting a container is like creating an image from a file system.

It's not possible to relaunch a container directly from another exported container.
You must first create an image with import.

###

for image
docker image save/load <-> tar

$ docker build -t mynode:0.1 .

$ docker image ls
REPOSITORY                                      TAG       IMAGE ID       CREATED         SIZE  
mynode                                          0.1       c68e7a86d468   8 seconds ago   48MB

$ docker image save -o monimage.tar mynode
(to compress with gzip: docker save mon_image | gzip > mon_image.tar.gz)

$ tar -tvf monimage.tar

$ docker image prune -a

$ docker image load < monimage.tar
or
$ docker image load -i mon_image.tar

$ docker run --rm mynode:0.1
hello node-test-012

###

Crypter ses identifiants GNU/Linux

$ sudo apt install pass

$ gpg2 --gen-key

Entrez bien votre nom et votre email lorsqu'ils sont demandés. Puis faites :

$ wget https://github.com/docker/docker-credential-helpers/releases/download/v0.6.3/docker-credential-pass-v0.6.3-amd64.tar.gz && tar -xf docker-credential-pass-v0.6.3-amd64.tar.gz && chmod +x docker-credential-pass && sudo mv docker-credential-pass /usr/local/bin/

$ pass init "VOTRE NOM

$ nano ~/.docker/config.json

Puis :

{
    "credsStore": "pass"
}

$ docker login

###

Push image to docker hub

$ docker login

$ docker build -t oldu73/mynode .

$ docker image push oldu73/mynode

$ docker image prune -a

$ docker run --rm oldu73/mynode

$ docker logout

###

Docker hub

docker image pull

docker image push

docker image pull/push <username>/<repertoire>:[tag]

docker search

https://hub.docker.com/

$ docker pull node

$ docker image ls
REPOSITORY                                      TAG       IMAGE ID       CREATED         SIZE
node                                            latest    7220633f01cd   7 days ago      992MB

$ docker run -it --rm node sh

# ls

# node -v1

# mkdir app

# cd app

# echo "console.log('Hello, world!');" > app.js

# node app.js

---

Image Variants

$ docker pull node:slim

node:latest = 992MB

VS

node:slim = 242MB

###

tag to tag image after build

$ docker image tag mynode:latest mynode:1.0

###

history

Show the history of an image

$ docker history mynode:latest 
IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
1a8f184ef896   8 minutes ago   sh                                              42.4MB
14119a10abf4   2 months ago    /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
<missing>      2 months ago    /bin/sh -c #(nop) ADD file:aad4290d27580cc1a…   5.6MB

###

logs

$ docker container logs redis-4.0-001

-f to follow
-t timestamp

###

COMMIT

Snapshot a container to image

$ docker run -it alpine sh

# mkdir app

open a new terminal to copy app.js "manually" to app folder

$ docker container cp app.js ed6:/app/

then from inside container install node.js

# apk add --update nodejs

then quit container and from host terminal

$ docker container commit -c 'CMD ["node", "/app/app.js"]' ed6 mynode

$ docker image ls
REPOSITORY                                      TAG       IMAGE ID       CREATED          SIZE
mynode                                          latest    1a8f184ef896   6 seconds ago    48MB
..

$ docker run --rm mynode
hello node-test-010

###

LABEL to add meta information

LABEL MAINTAINER=oldu73@gmail.com
LABEL version=1.0

$ docker build -t node10 .

$ docker image inspect node10:latest | less

/oldu

..
    "MAINTAINER": "oldu73@gmail.com",
                "version": "1.0"
            }
..

###

ENV

key value

Usable in container, available as environment variable:

ENV environment=production

$ docker build -t node10 .

$ docker run -it node10 sh
/app # env
HOSTNAME=ac68969910b7
SHLVL=1
HOME=/root
environment=production
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/app

###

ARG

Argument available at build time only:

ARG folder
ARG file

then

WORKDIR $folder
COPY $file .

then

$ docker build --build-arg folder=/app --build-arg file=app.js -t node-test-008 .

then if you try to retriev ARGs by typing env inside the container
you do not retrieve it because they are available only at build time:

$ docker run --rm -it node-test-008 sh
/app # env
HOSTNAME=22cc31c49889
SHLVL=1
HOME=/root
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/app

You may also put a default value:

ARG folder=/app

then

$ docker build --build-arg file=app.js -t node-test-008 .

###

You can still override the entry point with the --entrypoint option:

$ docker run --rm --entrypoint="echo" node:test "Hi, earth ;)"

or

$ docker run -it --entrypoint="/bin/sh" node:test

###

By default, Docker has a default entry point which is "/bin/sh -c" but does not have a default command.

$ man sh -> /-c

-c               Read commands from the command_string operand instead
                 of from the standard input.  Special parameter 0 will
                 be set from the command_name operand and the posi‐
                 tional parameters ($1, $2, etc.)  set from the re‐
                 maining argument operands.

###

May have ENTRYPOINT and CMD:

ENTRYPOINT ["echo"]
CMD ["hello"]

$ docker run --rm node:test                                               
hello

then you may override hello in run parameter:

$ docker run --rm node:test world
world

###

[] exec form -> recommended

.. shell form, like you would type the command in a terminal

###

ENTRYPOINT instead CMD avoid availability for end user to replace Dockerfile CMD by typing one
at the end of run terminal command:

ENTRYPOINT ["node", "app.js"]

in Dockerfile, then:

$ docker run --rm node:test
Bonjour

or

$ docker run --rm node:test echo test
Bonjour

same same ;-), echo test at the end is not taking under consideration

###

Typing a command at the end of the run command replace the one in Dockerfile:

$ docker run --rm node:test echo test
test

even if "CMD ["node", "app.js"]" in Dockerfile

###

Remove dangling (<none>) images:
$ docker image prune

For removing dangling and ununsed images:
$ docker image prune -a

###

Docker build not showing any output from commands(Dockerfile RUN):

Dockerfile

..
RUN echo hello

Don't show anything in console at build.

Use legacy mode by adding 'DOCKER_BUILDKIT=0' in front of docker build:

$ DOCKER_BUILDKIT=0 docker build -t test:latest .

..
Step 2/2 : RUN echo hello
 ---> Running in 3d9c96daa522
hello

or (new fashion) with "--progress=plain --no-cache" after build command:

$ docker build --progress=plain --no-cache -t node-test-007:latest .

#5 [3/5] RUN echo "Hello, world!"
#5 sha256:54040767d950b92027e2e377a0938fd42b89a34fa5d76e3ce281deacda0f1959
#5 0.281 Hello, world!
#5 DONE 0.3s

## List only container names

To list only names of all containers:

$ docker ps -a --format='{{.Names}}'

## ENV

'ENV', environment variable:

Dockerfile:
- Base image
- Test environment variable

FROM alpine

ENV DIR=/app
WORKDIR ${DIR}/back

then..

$ docker build -t node-test-006:latest .

then..

$ docker run -it node-test-006 sh

$c pwd
/app/back

## RUN

RUN exist in 'exec' and 'shell' mode (which is 'sh' by default).

exec:

RUN ["/bin/bash", "-c", "echo Bonjour !"]

shell:

RUN echo "Bonjour !"

## CMD

Remove CMD line to test container in interactive mode, then build:

$ docker build -t node-test-005:test .

Launch a container in interactive mode with sh as shell.
Don't forget to mention image tag after ':' as long as it ain't 'latest',
and to mention the shell at the end, 'sh':

$ docker run -it node-test-005:test sh
/app $c

As we can see, we are directly in 'app' folder.
And by typing 'echo $0' to check shell is indeed, 'sh':

$c echo $0
sh

And check 'node' version:

$c node --version
v14.18.1

And test 'app.js' (in app.js -> console.log('Hi test 005');):

$c node app.js
Hi test 005

## WORKDIR

WORKDIR define working directory in image:

WORKDIR /app

Then, for COPY command, no need to specify destination directory:

COPY ./app.js .

Also for CMD:

CMD ["node", "app.js"]

WORKDIR can be changed during the Dockerfile by being filled in again.

WORKDIR can create folders if they do not exist (this saves us a mkdir).

## FROM

Only one FROM command by Dockerfile

## VS Code Dockerfile command

VS Code, in a Dockerfile, hit ctrl+space to get a list of available commands.

Shortcut available due to Docker Microsoft extension installed in VS Code.

## ADD source destination

ADD source destination, similar to COPY but from URL or compressed file.

If it's a compressed file it will be automatically uncompressed.

## Copy context

Dockerfile context is current folder.

Could not COPY file from parent folder.

## Remove image with pattern

Remove all images that contain a pattern:

$ docker image rm $(docker images --format "{{.Repository}}" | grep node-test-00)

## Optimize cache

Optimizing cache.

Only the RUN, COPY, and ADD instructions create new layers and increase the size of an image.

It is therefore necessary to avoid multiplying the RUN commands, and try to group all the necessary
commands in a single RUN instruction (multi-line separator '\'):

FROM ubuntu
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y \
    git \
    nodejs \
    && rm -rf /var/lib/apt/lists/*

- It's recommended to put one installation by line, alphabetically sorted.

- !! It is mandatory to put apt-get update and apt-get install in the same RUN statement.
	Otherwise you will have serious cache problems.!!

- For images using Ubuntu or Debian, it is recommended to remove /var/lib/apt/ lists
	which contains the APT cache with all available packages in order to reduce the size of the image.

- ENV DEBIAN_FRONTEND=noninteractive allows us to specify to the Debian Package Manager (APT)
	that we are in a non-interactive environment for the installation.
	This avoids the prompts requested by some programs during installation (eg Git).

## Invalidate cache

!Important!

Invalidate cache at build.

If you have following instruction in Dockerfile it will be run only once at first build then cached:

RUN apt update && apt dist-upgrade -y

To not use a cache, you have to do:

docker build --no-cache -t test .

## Inspect Go template

docker inspect with Go template for format parameter.
e.g. to retrieve CMD:

$ docker inspect --format='{{.Config.Cmd}}' node-test-001
[node app/app.js]

###

Show the history of an image:

$ docker image history node-test-001

## Image size

node image = 900 MB/3 min VS alpine + node install = 50 MB/30 sec => ???

First build of node based image takes around 3 minutes.

Intermediate steps are cached by Docker.

Second build of node based image takes now only around 3 seconds.

## Dockerfile build image

Docker file, steps to build an image.

Instructions:
- FROM	// pull image from docker hub	\
											= layer
- RUN	// add node						/
> new image/running intermediate container
- COPY									/	= layer
> new image/running intermediate container
- CMD
> constructed image resulting from the different images, layers, and intermediate containers

You can't run commands in an image, so you need intermediate container.

## Dockerfile context

!! Warning !!

Create a Dockerfile then build image in a dedicated folder for your application
Otherwise, all files/folders contained in where you build image will be sent to the
daemon at build time as the context.

It is for this reason that you must create the Dockerfile in the folder of your application,
or here to test, in a separate folder.

If you create your Dockerfile directly in the root / directory,
your entire hard drive is sent as context to the daemon!

## APK

apk = Alpine Package Management

It is the equivalent of APT for Debian distributions
and therefore in particular for Ubuntu

apk add --update actually allows you to do apk update first, then apk add.

Debian equivalent of:

apt update && apt install

&& lets you do something based on whether the previous command completed successfully
- that's why you tend to see it chained as do_something && do_something_else_that_depended_on_something.

Furthermore, you also have || which is the logical or, and also ;
which is just a separator which doesn't care what happen to the command before.

## Dockerfile

Create a new folder docker-test.

Open it with VS Code.

In this example we gonna crate a node image (based on Alpine, not on official node image)
to simply test console log in a js file.

Create a new file named, with VS Code, 'app.js' and type in it:

console.log('Hello, world!');

Then, create a new file, in folder, with VS Code, named:

Dockerfile

Base image

Install node

Copy js file from local folder to container.

If folder does not exist, it will be created.

Type following commands in newly created Dockerfile file
(exactly respect the case and do not add any extensions):

FROM alpine

RUN apk add --update nodejs

COPY ./app.js /app/

## Node

Command to launch node with app.js in folder app
CMD ["node", "app/app.js"]

Then from terminal, build command with -t argument to name:tag image
. to mention path of Dockerfile in current directory:

$ docker build -t node-test-001:latest .

To check:

$ docker images (or $ docker image ls)

To test:

$ docker run node-test-001
Hello, world!

To auto delete container after execution, use –rm option:

$ docker run --rm node-test-001
Hello, world!

Check if node is installed on host machine:

$ node --version
v14.16.1

Test app.js on host machine (if node is installed):

$ node app.js 
Hello, world!

## VS Code

VS Code install Microsoft Docker extension

## Docker file in short

My image -> Docker file =:

- Base image

- Modification

- Action

## Add/del Alpine package

A base image (e.g. Alpine) is not based on any other image.

Add or del package in Alpine

$c apk update

$c apk add grep

$c apk del grep

## Running container as a running process

To demonstrate running container is just a running process on host machine

$ docker run -d redis

In contrary of a virtual machine (VM) a container is "just" a running process
sharing Linux kernel on host machine.

SRC: - https://stackoverflow.com/questions/64787125/why-doesnt-htop-show-my-docker-processes-using-wsl2
To see running process on WSL use command prompt (would be "$ sudo ps -ef | grep redis" on a Linux machine):
>wsl -d docker-desktop top
(or >wsl -d docker-desktop ps -ef)

If you want htop, you need to install it first:
>wsl -d docker-desktop apk update 
>wsl -d docker-desktop apk add htop

... 0% redis-server *:6379

To kill a process on host machine, WSL:
>wsl -d docker-desktop killall redis-server

To kill a process on host machine, Linux:
$ sudo killall redis-server

Running:
> wsl -d docker-desktop htop

See that container isn't running anymore:
$ docker container ls

***

// wip pointer

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
