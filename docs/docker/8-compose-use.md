# Compose - 8 - Use

Docker Compose - Use

***

## Introduction

Application =  
- Container Web server +  
- Container Database  

Setup:  
- Ports  
- Volumes  
- Network  
- Environment  

Docker Compose, talking about service.

One Application = (is composed of) Many Services (containers that communicate with each others).

Docker Compose is a CLI that read 'docker-compose.yml' file.

First, install Docker Compose and check installation and version by typing in a terminal:
```console
$ docker-compose version
```

***

## First use

'docker-compose ..' command(s) always refer to folder from where command is launched and context of 'docker-compose.yml' file contained in folder.

Yaml format configuration file.  
Yaml syntax is based on an indented key value format.
```console
 $ touch docker-compose.yml
```

First, mention version to use to ensure retro-compatibility.
To determine which version to specify in 'docker-compose.yml' file header, refer to docker engine version that run on your host machine:
```console
$ docker version
...
Server: Docker Engine - Community
 Engine:
  Version:          20.10.11
...
```

Then refer to documentation  
[Compose file - Reference and guidelines](https://docs.docker.com/compose/compose-file/)

Second, specify service(s).

docker-compose.yml:
```
version: '3.8'

services:
  myalpine:
    image: alpine
```

```console
$ docker-compose up
```

Alternative to go straight in service's container:
```console
$ docker-compose run myapline
$c
```

In another console:
```console
$ docker-compose ps
$ docker-compose ps -a
$ docker-compose down
```

Particularity of 'docker-compose down' command is to suppress (don't just stop) all container and network that was launched by previous 'docker-compose up' command.  
Anonymous volumes are never reused by Docker Compose. It launches new ones each time if declared in configuration.

Default command is the one defined in image, for 'alpine' it's '/bin/sh'.  
To overwrite default command, specify it in 'docker-compose.yml' file:
```
version: '3.8'

services:
  myalpine:
    image: alpine
    command: ls
```

Or by adding command directly after service name in run command:
```console
$ docker-compose run myalpine ls
```

Or with entry point in exec form (instead of shell) in 'docker-compose.yml' file:
```
version: '3.8'

services:
  myalpine:
    image: alpine
    entrypoint: ["ls"]
```

Or 'command: ["ls"]' instead of 'entrypoint: ["ls"]'

***

## Custom image

```console
$ touch Dockerfile
```

Dockerfile:
```
FROM alpine
CMD ["/bin/sh"]
```

docker-compose.yml
```
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    build: .
```

```console
$ docker-compose build
```

Have a look to VS Code Docker plugin to have a synthetic view of all Docker ecosystem components, containers, images, network, etc.

### Context and Dockerfile

Specify a context and Dockerfile:
```console
$ mkdir backend
$ cp Dockerfile backend/DockerfileBackend
```

docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    build:
      context: ./backend
      dockerfile: DockerfileBackend
```

### Arguments

Passing arguments, e.g. create a folder at build, 'Dockerfile' receive args from 'docker-compose.yml'.  
DockerfileBackend:
```
FROM alpine
ARG FOLDER
RUN mkdir $FOLDER
CMD ["/bin/sh"]
```

docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    build:
      context: ./backend
      dockerfile: DockerfileBackend
      args:
        - FOLDER=test
```

Note the 'arg' indentation with '-' for an array of values (yaml syntax).

test:
```console
$ docker-compose build
$ docker-compose run b
$c ls
.. test ..
```

Instead of list (- FOLDER=), e.g. for 'args' you may also use an object instead (FOLDER:).
docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    build:
      context: ./backend
      dockerfile: DockerfileBackend
      args:
        FOLDER: myfolder
```

test:
```console
$ docker-compose build
$ docker-compose run b
$c ls
.. myfolder ..
```

### Labels

docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    build:
      context: ./backend
      dockerfile: DockerfileBackend
      args:
        - FOLDER=test
      labels:
        - EMAIL=toto@test.com
```

test:
```console
$ docker-compose build
$ docker image inspect compose_b:latest | grep EMAIL
                "EMAIL": "toto@test.com"
```

***

## Ports

docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    build:
      context: ./backend
      dockerfile: DockerfileBackend
      args:
        FOLDER: myfolder
      labels:
        - EMAIL=toto@test.com
    ports:
        - 80:80
```

***

## Volumes

### Bind

```console
$ mkdir data
$ touch data/hello.txt
```

DockerfileBackend.yml:
```
FROM alpine
ARG FOLDER
WORKDIR /app
RUN mkdir $FOLDER
CMD ["/bin/sh"]
```

docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    build:
      context: ./backend
      dockerfile: DockerfileBackend
      args:
        FOLDER: myfolder
      labels:
        - EMAIL=toto@test.com
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./data
        target: /app/data
```

test:
```console
$ docker-compose build
$ docker-compose run b
$c cd data
$c ls
$c exit 
```

### Volumes

docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    build:
      context: ./backend
      dockerfile: DockerfileBackend
      args:
        FOLDER: myfolder
      labels:
        - EMAIL=toto@test.com
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./data
        target: /app/data
      - type: volume
        source: datavolume
        target: /app/datavolume

volumes:
  datavolume:
```

test:
```console
$ docker-compose build
$ docker-compose run b
[+] Running 1/0
 ⠿ Volume "compose_datavolume"  Created
$c ls
data        datavolume  myfolder
$c exit 
```

Volume option external to avoid docker-compose to create volume if it does not exist.  
docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    build:
      context: ./backend
      dockerfile: DockerfileBackend
      args:
        FOLDER: myfolder
      labels:
        - EMAIL=toto@test.com
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./data
        target: /app/data
      - type: volume
        source: datavolume
        target: /app/datavolume

volumes:
  datavolume:
    external: true
```

Before testing remove previously created volumes.  
test:
```console
$ docker-compose run b
external volume "" not found
```

To create anonymous volume, omit source option.  
docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    build:
      context: ./backend
      dockerfile: DockerfileBackend
      args:
        FOLDER: myfolder
      labels:
        - EMAIL=toto@test.com
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./data
        target: /app/data
      - type: volume
        source: datavolume
        target: /app/datavolume
      - type: volume
        target: /app/datavolumeanonymous

volumes:
  datavolume:
```

test:
```console
$ docker-compose build
$ docker-compose run b
$c ls
data                 datavolume           datavolumeanonymous  myfolder
```

Docker Compose does not always use the same anonymous volume for a service.  
Therefore, it is advisable to use:
```console
$ docker-compose down -v
```
to remove it.

-v, --volumes volumes, Remove named volumes declared in the volumes section of the Compose file and anonymous volumes attached to containers.

***

## Environment Variables

### from cli

```console
$ docker-compose run b
$c env
HOSTNAME=0b9907714155
SHLVL=1
HOME=/root
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/app
```

Add environnement variable from command line, value from host machine:
```console
$ docker-compose run -e USER b
$c env | grep USER
USER=toto
```

By default, by not specifying a value, docker-compose search on host machine environnement variable and if find one that match, pass it (e.g. here with USER that do exist on host machine and has a value).

Add environnement variable from command line with specified value:
```console
$ docker-compose run -e USER=tintin b
$c env | grep USER
USER=tintin
```

### from compose file

Without specifying a value (comes from host machine).  
docker-compose.yml:
```console
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    environment:
      - USER
    build:
      context: ./backend
      dockerfile: DockerfileBackend
      args:
        FOLDER: myfolder
      labels:
        - EMAIL=toto@test.com
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./data
        target: /app/data
      - type: volume
        source: datavolume
        target: /app/datavolume
      - type: volume
        target: /app/datavolumeanonymous

volumes:
  datavolume:
```

test:
```console
$ docker-compose build
$ docker-compose run b
$c env | grep USER
USER=toto
```

By specifying a value.  
docker-compose.yml:
```console
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    environment:
      - USER=tintin
    build:
      context: ./backend
      dockerfile: DockerfileBackend
      args:
        FOLDER: myfolder
      labels:
        - EMAIL=toto@test.com
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./data
        target: /app/data
      - type: volume
        source: datavolume
        target: /app/datavolume
      - type: volume
        target: /app/datavolumeanonymous

volumes:
  datavolume:
```

test:
```console
$ docker-compose build
$ docker-compose run b
$c env | grep USER
USER=tintin
```

### from .env file

If value of environment variable is not specified, docker compose search for corresponding value in host machine, if not found, search then in '.env' file.  
.env:
```
NODE_ENV=development
```

docker-compose.yml:
```console
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    environment:
      - NODE_ENV
    build:
      context: ./backend
      dockerfile: DockerfileBackend
      args:
        FOLDER: myfolder
      labels:
        - EMAIL=toto@test.com
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./data
        target: /app/data
      - type: volume
        source: datavolume
        target: /app/datavolume
      - type: volume
        target: /app/datavolumeanonymous

volumes:
  datavolume:
```

test:
```console
$ docker-compose build
$ docker-compose run b
$c env | grep NODE_ENV
NODE_ENV=development
```

By specifying an environnement file, all variables contained in it will be imported in container.  
.env:
```
NODE_ENV=development
TEST_ENV=test
```

docker-compose.yml:
```console
version: '3.8'

services:
  a:
    image: alpine
    command: ["ls"]
  b:
    env_file:
      - .env
    build:
      context: ./backend
      dockerfile: DockerfileBackend
      args:
        FOLDER: myfolder
      labels:
        - EMAIL=toto@test.com
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./data
        target: /app/data
      - type: volume
        source: datavolume
        target: /app/datavolume
      - type: volume
        target: /app/datavolumeanonymous

volumes:
  datavolume:
```

test:
```console
$ docker-compose build
$ docker-compose run b
$c env | grep _ENV
TEST_ENV=test
NODE_ENV=development
```

You may have many environnement files.  

You may specify env file in command line, works only with 'up':
```console
$ docker-compose --env-file ./.env up
```

You may use both 'env_file' and 'environnement' for same service.

You may specify compose project name instead of current folder with, e.g. in .env file:
```
COMPOSE_PROJECT_NAME=myproject
```

***

## Network

### Sub chapter y.1

...

***

## Chapter y

### Sub chapter y.1

...

***
