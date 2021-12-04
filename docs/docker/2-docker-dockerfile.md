# Docker - 2 - Docker File

***

## Image Variants

```
$ docker pull node:slim
```

node:latest = 992MB

VS

node:slim = 242MB

***

## tag

tag to tag image after build

```
$ docker image tag mynode:latest mynode:1.0
```

***

## history

Show the history of an image

```
$ docker history mynode:latest 
IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
1a8f184ef896   8 minutes ago   sh                                              42.4MB
14119a10abf4   2 months ago    /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
<missing>      2 months ago    /bin/sh -c #(nop) ADD file:aad4290d27580cc1a…   5.6MB
```

***

## logs

```
$ docker container logs redis-4.0-001
```

-f to follow
-t timestamp

***

## COMMIT

Snapshot a container to image

```
$ docker run -it alpine sh

# mkdir app
```

open a new terminal to copy app.js "manually" to app folder

```
$ docker container cp app.js ed6:/app/
```

then from inside container install node.js

```
$c apk add --update nodejs
```

then quit container and from host terminal

```
$ docker container commit -c 'CMD ["node", "/app/app.js"]' ed6 mynode

$ docker image ls
REPOSITORY                                      TAG       IMAGE ID       CREATED          SIZE
mynode                                          latest    1a8f184ef896   6 seconds ago    48MB
..

$ docker run --rm mynode
hello node-test-010
```

***

## LABEL

LABEL to add meta information

```
LABEL MAINTAINER=oldu73@gmail.com
LABEL version=1.0
```

```
$ docker build -t node10 .

$ docker image inspect node10:latest | less

/oldu

..
    "MAINTAINER": "oldu73@gmail.com",
                "version": "1.0"
            }
..
```

***

## ENV

key value

Usable in container, available as environment variable:

```
ENV environment=production
```

```
$ docker build -t node10 .

$ docker run -it node10 sh
/app $c env
HOSTNAME=ac68969910b7
SHLVL=1
HOME=/root
environment=production
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/app
```

***

## ARG

Argument available at build time only:

```
ARG folder
ARG file
```

then

```
WORKDIR $folder
COPY $file .
```

then

```
$ docker build --build-arg folder=/app --build-arg file=app.js -t node-test-008 .
```

then if you try to retriev ARGs by typing env inside the container
you do not retrieve it because they are available only at build time:

```
$ docker run --rm -it node-test-008 sh
/app # env
HOSTNAME=22cc31c49889
SHLVL=1
HOME=/root
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/app
```

You may also put a default value:

```
ARG folder=/app
```

then

```
$ docker build --build-arg file=app.js -t node-test-008 .
```

***

## Override entry point

You can still override the entry point with the --entrypoint option:

$ docker run --rm --entrypoint="echo" node:test "Hi, earth ;)"

or

$ docker run -it --entrypoint="/bin/sh" node:test

***

## Docker default entry point

By default, Docker has a default entry point which is "/bin/sh -c" but does not have a default command.

```
$ man sh -> /-c

-c

Read commands from the command_string operand instead
of from the standard input.  Special parameter 0 will
be set from the command_name operand and the positional
parameters ($1, $2, etc.)  set from the remaining argument operands.
```

***

## ENTRYPOINT and CMD

May have ENTRYPOINT and CMD:

```
ENTRYPOINT ["echo"]
CMD ["hello"]
```

```
$ docker run --rm node:test                                               
hello
```

then you may override hello in run parameter:

```
$ docker run --rm node:test world
world
```

***

## exec form

[] exec form -> recommended

.. shell form, like you would type the command in a terminal

***

## ENTRYPOINT instead CMD

ENTRYPOINT instead CMD avoid availability for end user to replace Dockerfile CMD by typing one
at the end of run terminal command:

```
ENTRYPOINT ["node", "app.js"]
```

in Dockerfile, then:

```
$ docker run --rm node:test
Bonjour
```

or

```
$ docker run --rm node:test echo test
Bonjour
```

same same ;-), echo test at the end is not taking under consideration

***

## Command at the end of run

Typing a command at the end of the run command replace the one in Dockerfile:

```
$ docker run --rm node:test echo test
test
```

even if "CMD ["node", "app.js"]" in Dockerfile

***

## Remove dangling images

Remove dangling (<none>) images:

```
$ docker image prune
```

For removing dangling and ununsed images:

```
$ docker image prune -a
```

***

## Docker build no output

Docker build not showing any output from commands(Dockerfile RUN):

Dockerfile

```
..
RUN echo hello
```

Don't show anything in console at build.

Use legacy mode by adding 'DOCKER_BUILDKIT=0' in front of docker build:

```
$ DOCKER_BUILDKIT=0 docker build -t test:latest .

..
Step 2/2 : RUN echo hello
 ---> Running in 3d9c96daa522
hello
```

or (new fashion) with "--progress=plain --no-cache" after build command:

```
$ docker build --progress=plain --no-cache -t node-test-007:latest .

[3/5] RUN echo "Hello, world!"
sha256:54040767d950b92027e2e377a0938fd42b89a34fa5d76e3ce281deacda0f1959
0.281 Hello, world!
DONE 0.3s
```

***

## List only container names

To list only names of all containers:

$ docker ps -a --format='{{.Names}}'

***

## ENV

'ENV', environment variable:

Dockerfile:
- Base image
- Test environment variable

```
FROM alpine

ENV DIR=/app
WORKDIR ${DIR}/back
```

then..

```
$ docker build -t node-test-006:latest .
```

then..

```
$ docker run -it node-test-006 sh

$c pwd
/app/back
```

***

## RUN

RUN exist in 'exec' and 'shell' mode (which is 'sh' by default).

exec:

```
RUN ["/bin/bash", "-c", "echo Bonjour !"]
```

shell:

```
RUN echo "Bonjour !"
```

***

## CMD

Remove CMD line to test container in interactive mode, then build:

```
$ docker build -t node-test-005:test .
```

Launch a container in interactive mode with sh as shell.
Don't forget to mention image tag after ':' as long as it ain't 'latest',
and to mention the shell at the end, 'sh':

```
$ docker run -it node-test-005:test sh
/app $c
```

As we can see, we are directly in 'app' folder.
And by typing 'echo $0' to check shell is indeed, 'sh':

```
$c echo $0
sh
```

And check 'node' version:

```
$c node --version
v14.18.1
```

And test 'app.js' (in app.js -> console.log('Hi test 005');):

```
$c node app.js
Hi test 005
```

***

## WORKDIR

WORKDIR define working directory in image:

```
WORKDIR /app
```

Then, for COPY command, no need to specify destination directory:

```
COPY ./app.js .
```

Also for CMD:

```
CMD ["node", "app.js"]
```

WORKDIR can be changed during the Dockerfile by being filled in again.

WORKDIR can create folders if they do not exist (this saves us a mkdir).

***

## FROM

Only one FROM command by Dockerfile

***

## VS Code Dockerfile command

VS Code, in a Dockerfile, hit ctrl+space to get a list of available commands.

Shortcut available due to Docker Microsoft extension installed in VS Code.

***

## ADD source destination

ADD source destination, similar to COPY but from URL or compressed file.

If it's a compressed file it will be automatically uncompressed.

***

## Copy context

Dockerfile context is current folder.

Could not COPY file from parent folder.

***

## Remove image with pattern

Remove all images that contain a pattern:

```
$ docker image rm $(docker images --format "{{.Repository}}" | grep node-test-00)
```

***

## Optimize cache

Optimizing cache.

Only the RUN, COPY, and ADD instructions create new layers and increase the size of an image.

It is therefore necessary to avoid multiplying the RUN commands, and try to group all the necessary
commands in a single RUN instruction (multi-line separator '\'):

```
FROM ubuntu
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y \
    git \
    nodejs \
    && rm -rf /var/lib/apt/lists/*
```

- It's recommended to put one installation by line, alphabetically sorted.

- !! It is mandatory to put apt-get update and apt-get install in the same RUN statement.
	Otherwise you will have serious cache problems.!!

- For images using Ubuntu or Debian, it is recommended to remove /var/lib/apt/ lists
	which contains the APT cache with all available packages in order to reduce the size of the image.

- ENV DEBIAN_FRONTEND=noninteractive allows us to specify to the Debian Package Manager (APT)
	that we are in a non-interactive environment for the installation.
	This avoids the prompts requested by some programs during installation (eg Git).

***

## Invalidate cache

!Important!

Invalidate cache at build.

If you have following instruction in Dockerfile it will be run only once at first build then cached:

RUN apt update && apt dist-upgrade -y

To not use a cache, you have to do:

docker build --no-cache -t test .

***

## Inspect Go template

docker inspect with Go template for format parameter.
e.g. to retrieve CMD:

```
$ docker inspect --format='{{.Config.Cmd}}' node-test-001
[node app/app.js]
```

***

###

Show the history of an image:

```
$ docker image history node-test-001
```

***

## Image size

node image = 900 MB/3 min VS alpine + node install = 50 MB/30 sec => ???

First build of node based image takes around 3 minutes.

Intermediate steps are cached by Docker.

Second build of node based image takes now only around 3 seconds.

***

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

***

## Dockerfile context

!! Warning !!

Create a Dockerfile then build image in a dedicated folder for your application
Otherwise, all files/folders contained in where you build image will be sent to the
daemon at build time as the context.

It is for this reason that you must create the Dockerfile in the folder of your application,
or here to test, in a separate folder.

If you create your Dockerfile directly in the root / directory,
your entire hard drive is sent as context to the daemon!

***

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

***

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

```
FROM alpine

RUN apk add --update nodejs

COPY ./app.js /app/
```

***
