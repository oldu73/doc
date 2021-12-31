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

## Chapter y

### Sub chapter y.1

...

***
