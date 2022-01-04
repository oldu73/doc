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

### Default

By default docker compose create a network with folder name as a prefix, or with value of 'COMPOSE_PROJECT_NAME' key in project's environnement variable:
```console
$ docker-compose up
[+] Running 4/4
 ⠿ Network myproject_default      Created
 ..
```

Note that containers using the network appear in the list ($ docker network inspect myproject_default) only when they are running.

Below, we make a test with ping, note that we use shell form (instead of exec (cause:  executable file not found in $PATH: unknown)).  
docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ping b
  b:
    command: ping a
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
$ docker-compose up
[+] Running 2/2
 ⠿ Container myproject_a_1  Recreated                                                                                                                 0.2s
 ⠿ Container myproject_b_1  Recreated                                                                                                                 0.2s 
Attaching to a_1, b_1
a_1  | PING b (172.29.0.3): 56 data bytes
b_1  | PING a (172.29.0.2): 56 data bytes
a_1  | 64 bytes from 172.29.0.3: seq=0 ttl=64 time=188.846 ms
b_1  | 64 bytes from 172.29.0.2: seq=0 ttl=64 time=0.152 ms
```

### Links

Links from a container to another one.  
docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ping b
  b:
    links:
      - "a:containerA"
    command: ping containerA
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
$ docker-compose up                                                                                       
[+] Running 3/2
 ⠿ Network myproject_default  Created                                                                                                                 0.0s 
 ⠿ Container myproject_a_1    Created                                                                                                                 0.8s 
 ⠿ Container myproject_b_1    Created                                                                                                                 0.1s
Attaching to a_1, b_1
b_1  | PING containerA (172.31.0.2): 56 data bytes
b_1  | 64 bytes from 172.31.0.2: seq=0 ttl=64 time=0.078 ms
b_1  | 64 bytes from 172.31.0.2: seq=1 ttl=64 time=0.190 ms
b_1  | 64 bytes from 172.31.0.2: seq=2 ttl=64 time=0.220 ms
a_1  | PING b (172.31.0.3): 56 data bytes
a_1  | 64 bytes from 172.31.0.3: seq=0 ttl=64 time=0.407 ms
b_1  | 64 bytes from 172.31.0.2: seq=3 ttl=64 time=0.305 ms
```

test to ping a and containerA from b:
```console
$ docker-compose up -d
$ docker-compose exec b sh
$c ping a
PING a (172.31.0.2): 56 data bytes
64 bytes from 172.31.0.2: seq=0 ttl=64 time=0.181 ms
..
$c ping containerA
PING containerA (172.31.0.2): 56 data bytes
64 bytes from 172.31.0.2: seq=0 ttl=64 time=0.287 ms
```

### Name

Give network a name to replace the default one.

docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ping b
  b:
    links:
      - "a:containerA"
    command: ping containerA
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
networks:
  default:
    name: mynetwork
```

test:
```console
$ docker-compose up
[+] Running 3/3
 ⠿ Network mynetwork        Created                                                                                                                0.0s
 ⠿ Container myproject_a_1  Created                                                                                                                0.8s
 ⠿ Container myproject_b_1  Created                                                                                                                2.4s
Attaching to a_1, b_1
b_1  | PING containerA (192.168.0.2): 56 data bytes
b_1  | 64 bytes from 192.168.0.2: seq=0 ttl=64 time=0.081 ms
b_1  | 64 bytes from 192.168.0.2: seq=1 ttl=64 time=0.088 ms
b_1  | 64 bytes from 192.168.0.2: seq=2 ttl=64 time=0.122 ms
a_1  | PING b (192.168.0.3): 56 data bytes
a_1  | 64 bytes from 192.168.0.3: seq=0 ttl=64 time=0.339 ms
```

### Networks

Link container to many networks with adding list in service configuration.  
docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ping b

  b:
    links:
      - "a:containerA"
    command: ping containerA
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
    networks:
      - 'othernetwork'

volumes:
  datavolume:
networks:
  default:
    name: mynetwork
```

test:
```console
$ docker-compose up
service "b" refers to undefined network othernetwork: invalid compose project
```

Error due to othernetwork missing.  
We add it to networks section in configuration file and then to services.  
docker-compose.yml:
```
version: '3.8'

services:
  a:
    image: alpine
    command: ping b
    networks:
      - 'othernetwork'

  b:
    links:
      - "a:containerA"
    command: ping containerA
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
    networks:
      - 'othernetwork'

volumes:
  datavolume:
networks:
  default:
    name: mynetwork
  othernetwork:
    driver: bridge
```

test:
```console
$ docker-compose up
[+] Running 3/3
 ⠿ Network myproject_othernetwork  Created                                                                                                          0.0s 
 ⠿ Container myproject_a_1         Created                                                                                                          0.1s 
 ⠿ Container myproject_b_1         Created                                                                                                          0.1s 
Attaching to a_1, b_1
b_1  | PING containerA (192.168.48.2): 56 data bytes
b_1  | 64 bytes from 192.168.48.2: seq=0 ttl=64 time=0.122 ms
b_1  | 64 bytes from 192.168.48.2: seq=1 ttl=64 time=0.051 ms
b_1  | 64 bytes from 192.168.48.2: seq=2 ttl=64 time=0.196 ms
a_1  | PING b (192.168.48.3): 56 data bytes
a_1  | 64 bytes from 192.168.48.3: seq=0 ttl=64 time=0.252 ms
b_1  | 64 bytes from 192.168.48.2: seq=3 ttl=64 time=0.200 ms
```

***

## Sample application

Node.js application that increment a counter in a MongoDB.

### MongoDB

We provide volume to handle db data, so, preamble is to "manually" create the needed volume:
```console
$ docker volume create mydb
```

docker-compose.yml:
```
version: '3.8'

services:
  db:
    image: mongo
    volumes:
      - type: volume
        source: mydb
        target: /data/db
volumes:
  mydb:
    external: true
```

We run db individually to initialize it:
```console
$ docker-compose run -d db
39cf..
$ docker container exec -it 39cf sh
$c mongo
> use test
> db.count.insertOne({ count: 0 })
{
        "acknowledged" : true,
        "insertedId" : ObjectId("61d1a03ac9a303a408034aca")
}
> db.count.findOne()
{ "_id" : ObjectId("61d1a03ac9a303a408034aca"), "count" : 0 }
> exit
bye
$c exit
$ docker container stop 39cf
```

In MongoDB, volume that contain the database may not be mounted anywhere.  
MongoDB will specifically search for database in '/data/db' folder.

No need to open specific port(s) for containers that run on same network.  
By default all ports are available for containers that run on same network.

### Node.js

Dockerfile:
```
FROM node:alpine
WORKDIR /app
COPY ./package.json .
RUN npm install
COPY . .
ENV PATH=$PATH:/app/node_modules/.bin
CMD ["nodemon", "-L", "src/app.js"]
```

docker-compose.yml:
```
version: '3.8'

services:

  db:
    image: mongo
    volumes:
      - type: volume
        source: mydb
        target: /data/db

  server:
    build: .
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./src
        target: /app/src

volumes:
  mydb:
    external: true
```

\src\app.js:
```javascript
require( 'console-stamp' )( console );  // to add timestamp in logs

const express = require("express");

const MongoClient = require('mongodb').MongoClient;

let count;

MongoClient.connect('mongodb://db', { useUnifiedTopology: true }, (err, client) => {
  if (err) {
    console.log(err);
  } else {
    console.log('CONNEXION DB OK!');
    count = client.db('test').collection("count");
  }
});

const app = express();

app.get('/', (req, res) => {
  console.log('request url: ' + req.url);
  count.findOneAndUpdate({}, { $inc: { count: 1 } }, { returnNewDocument: true }).then((doc) => {
    const value = doc.value;
    res.status(200).json(value.count);
  })
});

app.get('*', (req, res) => {
  res.end();
});

app.listen(80);
```

In a terminal:
```console
$ docker-compose up
..
server_1  | [02.01.2022 17:10.30.697] [LOG]   CONNEXION DB OK!
..
```

In a browser:
```
http://localhost/
```

### Authentication

We add authentication through environnement variable to MongoDB.

Clear docker environnement and recreate database volume:
```console
$ docker container prune
$ docker volume prune
$ docker volume create mydb
```

[Have a look to MongoDB official image on Docker Hub](https://hub.docker.com/_/mongo#:~:text=mongo/mongod.conf-,Environment%20Variables,-When%20you%20start)  
What's interesting us here is to set the two following environnement variables:  
- MONGO_INITDB_ROOT_USERNAME  
- MONGO_INITDB_ROOT_PASSWORD  

```console
$ touch .env
```

.env:
```
MONGO_INITDB_ROOT_USERNAME=toto
MONGO_INITDB_ROOT_PASSWORD=123
```

docker-compose.yml:
```
version: '3.8'

services:

  db:
    environment:
      - MONGO_INITDB_ROOT_USERNAME
      - MONGO_INITDB_ROOT_PASSWORD
    image: mongo
    volumes:
      - type: volume
        source: mydb
        target: /data/db

  server:
    build: .
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./src
        target: /app/src

volumes:
  mydb:
    external: true
```

In a terminal, set up db with authenticated user and then create a new user 'tintin' with password '456' and role 'readWrite' on db 'test':
```console
$ docker-compose run -d db
bad88..
$ docker exec -it bad88 sh
$c mongo
> use test
> db.count.insertOne({ count: 0 })
.. error.. command insert requires authentication..
> use admin
> db.auth({ user: 'toto', pwd: '123' })
1
> use test
> db.count.insertOne({ count: 0 })
{
        "acknowledged" : true,
        "insertedId" : ObjectId("61d1e40276df2cfd1b903a8f")
}
> db.count.findOne()
{ "_id" : ObjectId("61d1e40276df2cfd1b903a8f"), "count" : 0 }
> use admin
switched to db admin
> db.createUser({ user: 'tintin', pwd: '456', roles: [{ role: 'readWrite', db: 'test' }] })
Successfully added user: {
        "user" : "tintin",
        "roles" : [
                {
                        "role" : "readWrite",
                        "db" : "test"        
                }
        ]
}
> exit
bye
$c exit
$ docker stop bad88
```

Check that connection to db is OK ('CONNECTION DB OK!' in logs), but we cannot access data (trying to refresh 'localhost' in Internet browser), due to unauthenticated connection:
```console
$ docker-compose up
..
server_1  | [02.01.2022 19:47.14.606] [LOG]   CONNECTION DB OK!
..
server_1  | MongoError: command findAndModify requires authentication
..
```

We can authenticate with many different ways.

By specifying (hard coded) user password directly in 'app.js' file ('mongodb://tintin:456@db').  
We also add a 'console.log(process)' to have environnement variables in logs.  
app.js:
```javascript
require( 'console-stamp' )( console );  // to add timestamp in logs

const express = require("express");

const MongoClient = require('mongodb').MongoClient;

let count;

console.log(process) // to have environnement variables in logs

MongoClient.connect('mongodb://tintin:456@db', { useUnifiedTopology: true }, (err, client) => {
  if (err) {
    console.log(err);
  } else {
    console.log('CONNECTION DB OK!');
    count = client.db('test').collection("count");
  }
});

const app = express();

app.get('/', (req, res) => {
  console.log('request url: ' + req.url);
  count.findOneAndUpdate({}, { $inc: { count: 1 } }, { returnNewDocument: true }).then((doc) => {
    const value = doc.value;
    res.status(200).json(value.count);
  })
});

app.get('*', (req, res) => {
  res.end();
});

app.listen(80);
```

server_1 logs (to see environnement variables):
```console
..
server_1  |   env: {
server_1  |     NODE_VERSION: '17.3.0',
server_1  |     HOSTNAME: 'fbdb8ee68893',
server_1  |     YARN_VERSION: '1.22.17',
server_1  |     SHLVL: '1',
server_1  |     HOME: '/root',
server_1  |     PATH: '/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/app/node_modules/.bin',
server_1  |     PWD: '/app'
server_1  |   }
..
```

By refreshing Internet browser's page at localhost address, we may observer that application is now running fine.

Now we stop application by hitting 'Ctrl+c'.

Below is described a second manner (more secure, therefor, advised to use) to authenticate to 'db' service from 'sever' service through environnement variables.

.env:
```
MONGO_INITDB_ROOT_USERNAME=toto
MONGO_INITDB_ROOT_PASSWORD=123

MONGO_USER_NAME=tintin
MONGO_USER_PASSWORD=456
```

docker-compose.yml:
```
version: '3.8'

services:

  db:
    environment:
      - MONGO_INITDB_ROOT_USERNAME
      - MONGO_INITDB_ROOT_PASSWORD
    image: mongo
    volumes:
      - type: volume
        source: mydb
        target: /data/db

  server:
    environment:
      - MONGO_USER_NAME
      - MONGO_USER_PASSWORD
    build: .
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./src
        target: /app/src

volumes:
  mydb:
    external: true
```

app.js:
```javascript
require( 'console-stamp' )( console );  // to add timestamp in logs

const express = require("express");

const MongoClient = require('mongodb').MongoClient;

let count;

console.log(process.env) // to have environnement variables in logs

MongoClient.connect(`mongodb://${ process.env.MONGO_USER_NAME }:${ process.env.MONGO_USER_PASSWORD }@db`, { useUnifiedTopology: true }, (err, client) => {
  if (err) {
    console.log(err);
  } else {
    console.log('CONNECTION DB OK!');
    count = client.db('test').collection("count");
  }
});

const app = express();

app.get('/', (req, res) => {
  console.log('request url: ' + req.url);
  count.findOneAndUpdate({}, { $inc: { count: 1 } }, { returnNewDocument: true }).then((doc) => {
    const value = doc.value;
    res.status(200).json(value.count);
  })
});

app.get('*', (req, res) => {
  res.end();
});

app.listen(80);
```

! Be aware of literal evaluation with use of ` character to surround mongodb connection URL instead of ' character like before.

To test, type below command in a terminal and refresh Internet browser's page at 'localhost' address:
```console
$ docker-compose up
..
server_1  | [02.01.2022 20:32.25.069] [LOG]   {
server_1  |   NODE_VERSION: '17.3.0',
server_1  |   HOSTNAME: 'e9e18205ee72',
server_1  |   YARN_VERSION: '1.22.17',
server_1  |   SHLVL: '1',
server_1  |   HOME: '/root',
server_1  |   PATH: '/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/app/node_modules/.bin',
server_1  |   MONGO_USER_PASSWORD: '456',
server_1  |   PWD: '/app',
server_1  |   MONGO_USER_NAME: 'tintin'
server_1  | }
..
```

***

## Depends and Restart

### Depends

Specify containers up priority order (depends_on).  
E.g. in our application we want that server (Node.js application) start only once database (MongoDB) container is up.

docker-compose.yml:
```
version: '3.8'

services:

  db:
    environment:
      - MONGO_INITDB_ROOT_USERNAME
      - MONGO_INITDB_ROOT_PASSWORD
    image: mongo
    volumes:
      - type: volume
        source: mydb
        target: /data/db

  server:
    environment:
      - MONGO_USER_NAME
      - MONGO_USER_PASSWORD
    build: .
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./src
        target: /app/src
    depends_on:
      - db

volumes:
  mydb:
    external: true
```

In a terminal, type below command and you may observe, in logs, that, first, we have database logs, then server logs:
```console
$ docker-compose up

Attaching to db_1, server_1
db_1      | ..
db_1      | ..
..

server_1  | ..
server_1  | ..
..
```

### Restart

restart:  
- **"no"** (between quotes because without it has a yaml signification), default value, never restart automatically.  
- **always** restart if container stop from inside or Docker daemon restart, but not with a 'docker-compose stop' command.  
- **on-failure** restart container only on quit with error code.  
- **unless-stopped** always restart unless stopped manually with a 'docker-compose stop' command.  

To test it we add a '/err' route to the server to exit the process (process.exit(errorCode)).  
app.js:
```javascript
require( 'console-stamp' )( console );  // to add timestamp in logs

const express = require("express");

const MongoClient = require('mongodb').MongoClient;

let count;

console.log(process.env) // to have environnement variables in logs

MongoClient.connect(`mongodb://${ process.env.MONGO_USER_NAME }:${ process.env.MONGO_USER_PASSWORD }@db`, { useUnifiedTopology: true }, (err, client) => {
  if (err) {
    console.log(err);
  } else {
    console.log('CONNECTION DB OK!');
    count = client.db('test').collection("count");
  }
});

const app = express();

app.get('/err', (req, res) => {
  process.exit(0);
});

app.get('/', (req, res) => {
  console.log('request url: ' + req.url);
  count.findOneAndUpdate({}, { $inc: { count: 1 } }, { returnNewDocument: true }).then((doc) => {
    const value = doc.value;
    res.status(200).json(value.count);
  })
});

app.get('*', (req, res) => {
  res.end();
});

app.listen(80);
```

For testing purpose (different restart modes) avoid using nodemon.  
Dockerfile:
```
FROM node:alpine
WORKDIR /app
COPY ./package.json .
RUN npm install
COPY . .
ENV PATH=$PATH:/app/node_modules/.bin
CMD ["node", "src/app.js"]
```

Terminal:
```console
$ docker-compose down
$ docker-compose build --no-cache
```

#### no

docker-compose.yml (restart: "no"):
```
version: '3.8'

services:

  db:
    environment:
      - MONGO_INITDB_ROOT_USERNAME
      - MONGO_INITDB_ROOT_PASSWORD
    image: mongo
    volumes:
      - type: volume
        source: mydb
        target: /data/db

  server:
    environment:
      - MONGO_USER_NAME
      - MONGO_USER_PASSWORD
    build: .
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./src
        target: /app/src
    depends_on:
      - db
    restart: "no"

volumes:
  mydb:
    external: true
```

Test:
```console
$ docker-compose down
$ docker-compose up
```

Navigate in Internet browser to:
```
http://localhost/err
```

You may observe in logs:
```console
..
server_1 exited with code 0
..
```

In another terminal:
```console
$ docker ps -a
CONTAINER ID   IMAGE                COMMAND                  CREATED              STATUS
63c2040a78c6   node-server_server   "docker-entrypoint.s…"   About a minute ago   Exited (0) About a minute ago     
2d8b795bf1ed   mongo                "docker-entrypoint.s…"   7 minutes ago        Up About a minute
```

And server isn't available anymore.

#### always

docker-compose.yml (restart: always):
```
version: '3.8'

services:

  db:
    environment:
      - MONGO_INITDB_ROOT_USERNAME
      - MONGO_INITDB_ROOT_PASSWORD
    image: mongo
    volumes:
      - type: volume
        source: mydb
        target: /data/db

  server:
    environment:
      - MONGO_USER_NAME
      - MONGO_USER_PASSWORD
    build: .
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./src
        target: /app/src
    depends_on:
      - db
    restart: always

volumes:
  mydb:
    external: true
```

Terminal:
```console
$ docker-compose up
```

In an Internet browser, navigate alternatively to following addresses and observe in logs server exit and restart automatically:
```
http://localhost/
http://localhost/err
```

Terminal:
```console
..
server_1  | [03.01.2022 16:24.28.780] [LOG]   CONNECTION DB OK!
..
server_1 exited with code 0
..
server_1  | [03.01.2022 16:27.10.339] [LOG]   CONNECTION DB OK!
..
```

In another terminal:
```console
$ docker-compose ps
NAME                   COMMAND                  SERVICE             STATUS              PORTS
node-server_db_1       "docker-entrypoint.s…"   db                  running             27017/tcp
node-server_server_1   "docker-entrypoint.s…"   server              running             0.0.0.0:80->80/tcp
```

If we "manually" stop 'server' container from another terminal with below command:
```console
$ docker-compose stop server
```

And then restart the Docker daemon, we may observe the 'server' that has restart automatically due to his 'always' restart policy and 'db' is down because, for now, he hasn't any restart policy defined in docker-compose yaml configuration file.

#### on-failure

docker-compose.yml (restart: on-failure):
```
version: '3.8'

services:

  db:
    environment:
      - MONGO_INITDB_ROOT_USERNAME
      - MONGO_INITDB_ROOT_PASSWORD
    image: mongo
    volumes:
      - type: volume
        source: mydb
        target: /data/db

  server:
    environment:
      - MONGO_USER_NAME
      - MONGO_USER_PASSWORD
    build: .
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./src
        target: /app/src
    depends_on:
      - db
    restart: on-failure

volumes:
  mydb:
    external: true
```

Terminal:
```console
$ docker-compose down
$ docker-compose up -d
$ docker-compose ps
NAME                   COMMAND                  SERVICE             STATUS              PORTS
node-server_db_1       "docker-entrypoint.s…"   db                  running             27017/tcp
node-server_server_1   "docker-entrypoint.s…"   server              running             0.0.0.0:80->80/tcp
```

Browse to:
```
http://localhost/err
```

In app.js exit code is '0' = no error, therefore container does not restart automatically:
```console
$ docker-compose ps
NAME                   COMMAND                  SERVICE             STATUS              PORTS
node-server_db_1       "docker-entrypoint.s…"   db                  running             27017/tcp
node-server_server_1   "docker-entrypoint.s…"   server              exited (0)
```

Modify exit code of 'err' route to app.js with '1', app.js:
```javascript
require( 'console-stamp' )( console );  // to add timestamp in logs

const express = require("express");

const MongoClient = require('mongodb').MongoClient;

let count;

console.log(process.env) // to have environnement variables in logs

MongoClient.connect(`mongodb://${ process.env.MONGO_USER_NAME }:${ process.env.MONGO_USER_PASSWORD }@db`, { useUnifiedTopology: true }, (err, client) => {
  if (err) {
    console.log(err);
  } else {
    console.log('CONNECTION DB OK!');
    count = client.db('test').collection("count");
  }
});

const app = express();

app.get('/err', (req, res) => {
  process.exit(1);
});

app.get('/', (req, res) => {
  console.log('request url: ' + req.url);
  count.findOneAndUpdate({}, { $inc: { count: 1 } }, { returnNewDocument: true }).then((doc) => {
    const value = doc.value;
    res.status(200).json(value.count);
  })
});

app.get('*', (req, res) => {
  res.end();
});

app.listen(80);
```

Terminal:
```console
$ docker-compose down
$ docker-compose up -d
$ docker-compose ps
NAME                   COMMAND                  SERVICE             STATUS              PORTS
node-server_db_1       "docker-entrypoint.s…"   db                  running             27017/tcp
node-server_server_1   "docker-entrypoint.s…"   server              running             0.0.0.0:80->80/tcp
```

Browse to:
```
http://localhost/err
```

In app.js exit code is '1' = error, therefore container restart automatically:
```console
$ docker-compose ps
NAME                   COMMAND                  SERVICE             STATUS              PORTS
node-server_db_1       "docker-entrypoint.s…"   db                  running             27017/tcp
node-server_server_1   "docker-entrypoint.s…"   server              running             0.0.0.0:80->80/tcp
```

#### unless-stopped

Will always restart except if stopped "manually" with 'docker-compose stop server', then restart Docker daemon, then stopped 'server' container will not restart.

docker-compose.yml (restart: unless-stopped):
```
version: '3.8'

services:

  db:
    environment:
      - MONGO_INITDB_ROOT_USERNAME
      - MONGO_INITDB_ROOT_PASSWORD
    image: mongo
    volumes:
      - type: volume
        source: mydb
        target: /data/db

  server:
    environment:
      - MONGO_USER_NAME
      - MONGO_USER_PASSWORD
    build: .
    ports:
      - 80:80
    volumes:
      - type: bind
        source: ./src
        target: /app/src
    depends_on:
      - db
    restart: unless-stopped

volumes:
  mydb:
    external: true
```

Terminal:
```console
$ docker-compose down
$ docker-compose up -d
$ docker-compose ps
NAME                   COMMAND                  SERVICE             STATUS              PORTS
node-server_db_1       "docker-entrypoint.s…"   db                  running             27017/tcp
node-server_server_1   "docker-entrypoint.s…"   server              running             0.0.0.0:80->80/tcp
$ docker-compose stop server
$ docker-compose ps
NAME                   COMMAND                  SERVICE             STATUS              PORTS
node-server_db_1       "docker-entrypoint.s…"   db                  running             27017/tcp
node-server_server_1   "docker-entrypoint.s…"   server              exited (137)
```

Restart Docker daemon, then:
```console
$ docker-compose ps
NAME                   COMMAND                  SERVICE             STATUS              PORTS
node-server_db_1       "docker-entrypoint.s…"   db                  exited (255)        27017/tcp
node-server_server_1   "docker-entrypoint.s…"   server              exited (137)
```

We may observe that 'server' container hasn't restarted automatically due to fact it has been "manually" stopped before Docker daemon restart.

Note that you can change the restart configuration of an already running container by doing:
```console
$ docker container update --restart unless-stopped ID
```

***

## Other commands

First:
```console
$ docker-compose up -d
```

### logs

View output from containers:
```console
$ docker-compose logs
..
db_1  | ..
server_1  | ..
..
```

To follow -f option, to show timestamps -t options:
```console
$ docker-compose logs -f -t
```

'Ctrl+c' does not stop containers, stop only logs display:
```console
$ Ctrl+c
$ docker-compose ps
NAME                   COMMAND                  SERVICE             STATUS              PORTS
node-server_db_1       "docker-entrypoint.s…"   db                  running             27017/tcp
node-server_server_1   "docker-entrypoint.s…"   server              running             0.0.0.0:80->80/tcp
```

### top

Display the running processes:
```console
$ docker-compose top
```

### misc

Stop a container:
```console
$ docker-compose stop server

$ docker-compose ps
NAME                   COMMAND                  SERVICE             STATUS              PORTS
node-server_db_1       "docker-entrypoint.s…"   db                  running             27017/tcp
node-server_server_1   "docker-entrypoint.s…"   server              exited (137)
```

Remove a stopped container:
```console
$ docker-compose rm server

$ docker-compose ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
node-server_db_1    "docker-entrypoint.s…"   db                  running             27017/tcp
```

Remove a running container (+ -f to avoid confirm's need):
```console
$ docker-compose rm -s server
```

Remove anonym volumes belonging to container:
```console
$ docker-compose rm -v db
```

After removing a container, to get it back (+ -d):
```console
$ docker-compose up
```

Port mapping information and entering ip allowed (0.0.0.0 for all entering ip address allowed):
```console
$ docker-compose port server 80
0.0.0.0:80
```

Means, outside port 80 is mapped to inside port 80 and all ip addresses allowed.

### config

Let see environnement variables replaced with found values and configuration that will then be used to build the stack:
```console
$ docker-compose config

services:
  db:
    environment:
      MONGO_INITDB_ROOT_PASSWORD: "123"
      MONGO_INITDB_ROOT_USERNAME: toto
    image: mongo
    restart: unless-stopped
    volumes:
    - type: volume
      source: mydb
      target: /data/db
  server:
    build:
      context: .
    depends_on:
      db:
        condition: service_started
    environment:
      MONGO_USER_NAME: tintin
      MONGO_USER_PASSWORD: "456"
    ports:
    - mode: ingress
      target: 80
      published: 80
      protocol: tcp
    restart: unless-stopped
    volumes:
    - type: bind
      source: /mnt/c/git/doc/test/docker/node-server/src
      target: /app/src
volumes:
  mydb:
    name: mydb
    external: true
```

### pull push

pull: get latest images of containers that compose the stack.
push: if image has been modified with custom Dockerfile, let us push it to Docker Hub.

***
