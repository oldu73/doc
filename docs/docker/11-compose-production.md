# Compose - 11 - Production

Docker Compose - Production

## Introduction

Almost same components as the ones presented/used in preceding chapter [Docker Compose - Services](10-compose-services.md).

For this note (Compose - 11 - Production), we refer to folder architecture and contained files from chapter [Docker Compose - Services](10-compose-services.md).

This time all those component are going to be set to work in production, on a real server.

### Project components

Introduction to project components:

- [Certbot](https://certbot.eff.org/), to get your site on https:// (encrypted communications) with TLS certificate (little lock before address in Internet browser).  
- NGINX (Http request and reverse proxy).  
- React (Application), could be Vue.js or Angular, would be the same.  
- [PM2](https://pm2.keymetrics.io/), process manager (load balancer) for Node.js.  
- Node.js (API), to get better performance it's advised to get one running instance of Node.js by CPU core (on host server).  
- MongoDB (database), only one instance here (scale is out of scope here).

- [OVHcloud](https://www.ovhcloud.com/fr/) (VPS, virtual private server and DNS).
- [GitLab](https://about.gitlab.com/), code hosting.

### Project architecture

Below is a detailed schema of proposed architecture, obviously, everything in dedicated Docker containers:

```text
                          React (build)
                         /
                        /
                       NGINX
                      /
                     /
                    !=/api
                   /
--Certbot--NGINX--/     
                  \
                   ==/api
                    \
                     \
                      PM2
                       \
                        ---------------
                                       |
                             ---------------------
                             |         |         |
                         Node.js   Node.js   Node.js
                             ↑         ↑         ↑
                             ↓         ↓         ↓
                             ---------------------
                             |     MongoDB       |
                             ---------------------
```

Real scalable and performing architecture for single physical server.

For huge application running on multiple physical server, see [Docker Swarm](https://docs.docker.com/engine/swarm/).

### SIGINT

SIGINT: - A signal sent to a process in order to cause it to be interrupted properly (graceful shutdown).

This is a bit off topic, but here we will properly handle the Node.js application (API) interrupt signal.

In file '.../fullstack/api/src/index.js':

```javascript
const express = require("express");

const MongoClient = require('mongodb').MongoClient;

let count;

const MongUrl = process.env.NODE_ENV === 'production' ? 
`mongodb://${ process.env.MONGO_USERNAME }:${ process.env.MONGO_PWD }@db` : 
`mongodb://db`

console.log(process.env) // to have environnement variables in logs

MongoClient.connect(MongUrl, { useUnifiedTopology: true }, (err, client) => {
  if (err) {
    console.log(err);
  } else {
    console.log('CONNECTION DB OK!');
    count = client.db('test').collection("count");
  }
});

const app = express();

app.get('/api/count', (req, res) => {
  console.log('request url: ' + req.url);
  count.findOneAndUpdate({}, { $inc: { count: 1 } }, { returnNewDocument: true }).then((doc) => {
    const value = doc.value;
    res.status(200).json(value.count);
  })
});

app.all('*', (req, res) => {
  res.status(404).end();
});

app.listen(80);

process.addListener('SIGINT', () => {
  console.log('Received interruption signal!');
})
```

In a terminal:

```console
docker-compose -f docker-compose.dev.yml build api
docker-compose -f docker-compose.dev.yml run api
CTRL + C
^CReceived interruption signal!
npm ERR! path /app
npm ERR! command failed
npm ERR! signal SIGINT
npm ERR! command sh -c nodemon ./src/index.js
```

It was just to observe in logs that process correctly receive interruption signal and then there are some red error messages.

Below is correct management of process interruption by closing database connection and server that return 1 on exit error.

In file '.../fullstack/api/src/index.js':

```javascript
const express = require("express");

const MongoClient = require('mongodb').MongoClient;

let clientDb;

let count;

const MongUrl = process.env.NODE_ENV === 'production' ? 
`mongodb://${ process.env.MONGO_USERNAME }:${ process.env.MONGO_PWD }@db` : 
`mongodb://db`

console.log(process.env) // to have environnement variables in logs

MongoClient.connect(MongUrl, { useUnifiedTopology: true }, (err, client) => {
  if (err) {
    console.log(err);
  } else {
    console.log('CONNECTION DB OK!');
    clientDb = client;
    count = client.db('test').collection("count");
  }
});

const app = express();

app.get('/api/count', (req, res) => {
  console.log('request url: ' + req.url);
  count.findOneAndUpdate({}, { $inc: { count: 1 } }, { returnNewDocument: true }).then((doc) => {
    const value = doc.value;
    res.status(200).json(value.count);
  })
});

app.all('*', (req, res) => {
  res.status(404).end();
});

const server = app.listen(80);

process.addListener('SIGINT', () => {
  console.log('Received interruption signal!');

  server.close((err) => {
    if (err) {
      process.exit(1);
    } else {
      if (clientDb) {
        clientDb.close((err) => process.exit(err ? 1 : 0));
      } else {
        process.exit(0);
      }
    }
  })
})
```

In production we also use LTS (Long-term support) version of alpine node image.

In '.../fullstack/api/Dockerfile' file:

```text
FROM node:lts-alpine
WORKDIR /app
COPY package.json .
RUN npm install
# To avoid 'EACCES: permission denied' issue on '/app/node_modules/.cache' folder
RUN mkdir -p node_modules/.cache && chmod -R 777 node_modules/.cache
COPY . .
EXPOSE 80
CMD ["npm", "start"]
```

In a terminal (from '.../fullstack' folder), to tests if it's running well.

```console
docker-compose -f docker-compose.dev.yml build api
docker-compose -f docker-compose.dev.yml run api
CTRL + C
```

Still experiment some red error messages in logs after SIGINT received by the process, maybe due to no database connection for now (let's see further on this chapter notes).

***

## Chapter y

### Sub chapter y.1

...

***

NGINX

React

Node.js

webpack

Docker Compose

MongoDB

Certbot

PM2
