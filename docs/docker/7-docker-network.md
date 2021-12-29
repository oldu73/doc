# Docker - 7 - Network

***

## Introduction

WAN = Internet  
LAN = Local

docker network:  
- ls  
- create  
- rm  
- inspect  
- connect  
- disconnect  
- prune  
- \--network \| \--net  

3 methods:  
- Bridge, sub-segment  
- Host, merge host machine network  
- Overlay, Docker Swarm  
(- MACVLAN)  
(- Others)  

### Bridge (mainly used)

Grouped by sub-segment.  
Docker as bridge manager.  
By default a container belongs to named "bridge" (Docker0) network.

### Host (Linux only, rarely used)

IP addresses for containers defined by router like for host machine.
Containers will be straight forward connected to local network.

### Overlay (Swarm)

To establish communication between Docker Daemons.

***

## Bridge
```console
$ docker network
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
266b7ae5e9d7   bridge    bridge    local
ada5f50a5c41   host      host      local
119b5f46e464   none      null      local

$ docker network inspect bridge
...
"Containers": {},
...

$ ifconfig
...
docker0:
...

$ docker run --rm -it alpine sh
```

in a second terminal
```console
$ docker network inspect bridge
...
"Containers": {
            "04d3d4540a17d21ea7db83779e8de1716e6e3a4122e1f2c2f66c60d25a094656": {
                "Name": "stoic_wozniak",
                "EndpointID": "2ed11c55d850ed3cc4eec221f705dad2a9679a016934cd991cd96da86d2dfcbd",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
}
...

$ docker run --rm -it alpine sh
```

in a third terminal
```console
$ docker network inspect bridge
"Containers": {
            "04d3d4540a17d21ea7db83779e8de1716e6e3a4122e1f2c2f66c60d25a094656": {  
                "Name": "stoic_wozniak",
                "EndpointID": "2ed11c55d850ed3cc4eec221f705dad2a9679a016934cd991cd96da86d2dfcbd",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            },
            "76ff8f56718ae5244eabe03092f7a0227aa2e42249bf2ab1c8f91a9faf76c715": {  
                "Name": "admiring_driscoll",
                "EndpointID": "684e6a570db46dadd9bdf53bf383ea069fa626fc16b7ab26c11e604415c00b25",
                "MacAddress": "02:42:ac:11:00:03",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            }
}
```

in second terminal
```console
$c ping google.ch
$c ping 172.17.0.2
```

Be aware that ip address maybe attributed randomly by Docker.  
To use name on default network, use **--name** and **--link** (deprecated) options on run command then you may ping by name instead of ip address (only for default bridge network).

in first terminal
```console
$ docker run --rm --name alpine1 -it alpine sh
```

in second terminal
```console
$ docker run --rm --link alpine1 -it alpine sh
$c ping alpine1
```

***

## Create bridge

Create a network, make two containers communicate through it and use container name instead of ip addresses.

Create network, default driver = bridge
```console
$ docker network create mynet

$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
266b7ae5e9d7   bridge    bridge    local
ada5f50a5c41   host      host      local
0a85e3670d62   mynet     bridge    local
119b5f46e464   none      null      local
```

Use network with a named (important for name resolution over the network) container

```console
$ docker run --rm --network mynet --name server1 -d alpine ping google.ch

$ docker inspect mynet

"Containers": {
            "249b952ab5db2ac4f3077e1a7fb89582eedaa02c236a92a7e15fc5cee73d3292": {
                "Name": "server1",
                "EndpointID": "48284c2119158a9ebf9d67b3eee0c74c58ede079ba173b769f5b791f2f507abb",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        }

$ docker run --rm -it --network mynet --name server2 alpine sh

$c ping server1
PING server1 (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.079 ms
64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.166 ms
```

From another terminal

```console
$ docker exec -it server1 sh
$c ping server2
PING server2 (172.18.0.3): 56 data bytes
64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.125 ms
64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.496 ms
```

Remove network
```console
$ docker network rm mynet
```

Remove all network at once
```console
$ docker network prune
```

***

## Connect a Node.js server with MongoDB

Goal: - display a counter in a browser at 'localhost' address.  

Architecture:  
- Image - MongoDB  
- Image - Node.js  
- Volumes - mydb { count: x }  
- Container - server  
- Container - db  
- Network - mynet  
- Port 80 open to listen to request (count++) from a browser at 'localhost' address  

### MongoDB
Volume and container
```console
$ docker volume create mydb
$ docker run --name db --mount type=volume,source=mydb,target=/data/db -d mongo
```

Network and connect (and disconnect from default bridge)
```console
$ docker network create mynet
$ docker network connect mynet db
$ docker network disconnect bridge db
```

Create db and collection to handle and initialize the counter
```console
$ docker exec -it db sh
$c mongo
> use test
switched to db test
> db.count.insertOne({count:0})
{
        "acknowledged" : true,
        "insertedId" : ObjectId("61cc2517094e32ba7f98bb31")
}
> db.count.find()
{ "_id" : ObjectId("61cc2517094e32ba7f98bb31"), "count" : 0 }
> exit
bye
$c exit
```

### Node server Development

#### Development Environnement setup

First step, application development with bind mount.

We should use the mongo javascript driver in our application to allow connection to the db.

In 'node-server' folder.

File 'package.json' add mongo dependencies (browse for "npm mongodb" -> MongoDB NodeJS Driver, to check version)
```json

{
    "dependencies": {
      "express": "^4.17.1",
      "mongodb": "^3.6.2",
      "nodemon": "^2.0.6",
      "console-stamp": "^3.0.3"
    }
}

```

"console-stamp" is to add timestamp in logs

^version “Compatible with version”, will update you to all future minor/patch versions, without incrementing the major version. ^2.3.4 will use releases from 2.3.4 to <3.0.0.

Dockerfile
```
FROM node:alpine
WORKDIR /app
COPY ./package.json .
RUN npm install
COPY . .
ENV PATH=$PATH:/app/node_modules/.bin
CMD ["nodemon", "-L", "src/app.js"]
```

Build image in 'node-server' folder
```console
$ docker build -t node-server .
```

Application is in js file 'node-server/src/app.js'
```javascript

const express = require("express");

const app = express();

app.get("*", (req, res) => {
  res.status(200).json("Hello, world!");
});

app.listen(80);

```

To develop application use a bind mount
```console
$ docker run --name server --mount type=bind,source="$(pwd)"/src,target=/app/src -p 80:80 --network mynet node-server
```

Browse to 'localhost'.  
Observe live change availability by editing "Hello, world!" response in 'app.js' file and refreshing 'localhost' page in internet browser.

Check that port '80' is published for 'server' container
```console
$ docker container port server
80/tcp -> 0.0.0.0:80
```

#### Development Server configuration

Modify 'app.js' file as follow
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

### Node server Production

Rebuild node server image with released app.js in it (above development has been erased by bind mount)

In 'node-server' folder
```console
$ docker build -t node-server .
```

If 'node-server' container is still running, remove it and then
```console
$ docker run --name server --network mynet -d -p 80:80 node-server
```

***

## Host

C.f. [Bridge section](##bridge) for initial setup.

Reset 'app.js' to
```javascript

const express = require("express");

const app = express();

app.get("*", (req, res) => {
  res.status(200).json("Hello, world!");
});

app.listen(80);

```

Rebuild 'node-server' image
```console
$ docker build -t node-server .
```

Relaunch server but on local network, this time, no need to publish port
```console
$ docker run --network host node-server
```

Do not work on WSL (Windows Subsystem for Linux), neither MacOs.

To not use any network
```console
$ docker run --network none node-server
```

***
