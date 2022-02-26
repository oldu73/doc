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

## PM2

One Node.js instance by CPU available threads.

Cluster module.

In fullstack/api folder, rename Dockerfile as Dockerfile.dev and create new Dockerfile.prod for production:

```console
mv Dockerfile Dockerfile.dev
touch Dockerfile.prod
```

Edit docker-compose.dev.yml accordingly to use Dockerfile.dev for api service:

```yaml
version: "3.8"
services:
  client:
    build:
      context: ./client
      dockerfile: Dockerfile.dev
    volumes:
      - type: bind
        source: ./client
        target: /app
      - type: volume
        target: /app/node_modules
    ports:
      - 3000:3000
  api:
    build:
      context: ./api
      dockerfile: Dockerfile.dev
    volumes:
      - type: bind
        source: ./api/src
        target: /app/src
    ports:
      - 3001:80
  db:
    image: mongo
    volumes:
      - type: volume
        source: dbtest
        target: /data/db
  reverse-proxy:
    build:
      context: ./reverse-proxy
      dockerfile: Dockerfile.dev
    ports:
      - 80:80
    depends_on:
      - api
      - db
volumes:
  dbtest:
```

Dockerfile.prod:

```text
FROM node:lts-alpine
WORKDIR /app
COPY package.json .
RUN npm install
RUN npm i pm2 -g
# To avoid 'EACCES: permission denied' issue on '/app/node_modules/.cache' folder
RUN mkdir -p node_modules/.cache && chmod -R 777 node_modules/.cache
COPY . .
EXPOSE 80
CMD [ "npm", "run", "prod" ]
```

package.json:

```json
{
  "name": "api",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "nodemon ./src/index.js",
    "prod": "pm2-runtime ecosystem.config.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.2",
    "mongodb": "^4.3.0",
    "nodemon": "^2.0.15"
  }
}

Still in fullstack/api folder create ecosystem.config.js file:

```console
touch ecosystem.config.js
```

ecosystem.config.js:

```javascript
module.exports = [
    {
        script: 'src/index.js',
        name: 'api',
        exec_mode: 'cluster',
        instances: 'max'
    }
]
```

In fullstack folder, edit docker-compose.prod.yml accordingly (for now, db dependency of api service is commented):

```yaml
version: '3.8'
services:
  client:
    build:
      context: ./client
      dockerfile: Dockerfile.prod
    restart: unless-stopped
  api:
    build:
      context: ./api
      dockerfile: Dockerfile.prod
    env_file:
      - ./api/.env
    environment:
      NODE_ENV: production
    restart: unless-stopped
    #depends_on:
    #  - db
  db:
    image: mongo
    volumes:
      - type: volume
        source: dbprod
        target: /data/db
    env_file:
      - ./db/.env
    restart: unless-stopped
  reverse-proxy:
    build:
      context: ./reverse-proxy
      dockerfile: Dockerfile.prod
    ports:
      - 80:80
    restart: unless-stopped
    depends_on:
      - api
      - db
      - client

volumes:
  dbprod:
    external: true
```

JS application is also commented, for now, to avoid issue trying db connection, .../fullstack/api/src/index.js:

```javascript
console.log('Hi, PM2!');

/*
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
*/
```

In fullstack folder, build, run production api (maybe dbprod volume is missing, so recreate it):

```console
docker volume create dbprod
docker-compose -f docker-compose.prod.yml build api
docker-compose -f docker-compose.prod.yml run api

> api@1.0.0 prod
> pm2-runtime ecosystem.config.js

2022-02-11T16:19:52: PM2 log: Launching in no daemon mode
2022-02-11T16:19:52: PM2 log: App [api:0] starting in -cluster mode-
2022-02-11T16:19:52: PM2 log: App [api:0] online
2022-02-11T16:19:52: PM2 log: App [api:1] starting in -cluster mode-
2022-02-11T16:19:52: PM2 log: App [api:1] online
2022-02-11T16:19:52: PM2 log: App [api:2] starting in -cluster mode-
2022-02-11T16:19:52: PM2 log: App [api:2] online
2022-02-11T16:19:52: PM2 log: App [api:3] starting in -cluster mode-
2022-02-11T16:19:52: PM2 log: App [api:3] online
2022-02-11T16:19:52: PM2 log: App [api:4] starting in -cluster mode-
2022-02-11T16:19:52: PM2 log: App [api:4] online
2022-02-11T16:19:52: PM2 log: App [api:5] starting in -cluster mode-
Hi, PM2!
Hi, PM2!
2022-02-11T16:19:52: PM2 log: App [api:5] online
2022-02-11T16:19:52: PM2 log: App [api:6] starting in -cluster mode-
Hi, PM2!
2022-02-11T16:19:52: PM2 log: App [api:6] online
2022-02-11T16:19:52: PM2 log: App [api:7] starting in -cluster mode-
Hi, PM2!
2022-02-11T16:19:52: PM2 log: App [api:7] online
Hi, PM2!
Hi, PM2!
Hi, PM2!
Hi, PM2!
```

Have a look to [Quick Start page of PM2](https://pm2.keymetrics.io/docs/usage/pm2-doc-single-page/)

In a second terminal exec a container sh session and list PM2 processes:

```console
docker exec -it fullstack_api_run_d97c9b2061d1 sh
pm2 ls
┌────┬────────────────────┬──────────┬──────┬───────────┬──────────┬──────────┐
│ id │ name               │ mode     │ ↺    │ status    │ cpu      │ memory   │
├────┼────────────────────┼──────────┼──────┼───────────┼──────────┼──────────┤
│ 0  │ api                │ cluster  │ 0    │ online    │ 0%       │ 44.4mb   │
│ 1  │ api                │ cluster  │ 0    │ online    │ 0%       │ 45.0mb   │
│ 2  │ api                │ cluster  │ 0    │ online    │ 0%       │ 44.9mb   │
│ 3  │ api                │ cluster  │ 0    │ online    │ 0%       │ 44.5mb   │
│ 4  │ api                │ cluster  │ 0    │ online    │ 0%       │ 45.0mb   │
│ 5  │ api                │ cluster  │ 0    │ online    │ 0%       │ 45.2mb   │
│ 6  │ api                │ cluster  │ 0    │ online    │ 0%       │ 45.3mb   │
│ 7  │ api                │ cluster  │ 0    │ online    │ 0%       │ 44.7mb   │
└────┴────────────────────┴──────────┴──────┴───────────┴──────────┴──────────┘
```

***

## Production Environment

Remove references to .env file for api and db service.

Values contained in those files (admin and user credentials) will be entered at server start.

.../fullstack/docker-compose.prod.yml:

```yaml
version: '3.8'
services:
  client:
    build:
      context: ./client
      dockerfile: Dockerfile.prod
    restart: unless-stopped
  api:
    build:
      context: ./api
      dockerfile: Dockerfile.prod
    environment:
      - MONGO_USERNAME
      - MONGO_PWD
      - NODE_ENV=production
    restart: unless-stopped
    #depends_on:
    #  - db
  db:
    image: mongo
    volumes:
      - type: volume
        source: dbprod
        target: /data/db
    environment:
      - MONGO_INITDB_ROOT_USERNAME
      - MONGO_INITDB_ROOT_PASSWORD
    restart: unless-stopped
  reverse-proxy:
    build:
      context: ./reverse-proxy
      dockerfile: Dockerfile.prod
    ports:
      - 80:80
    restart: unless-stopped
    depends_on:
      - api
      - db
      - client

volumes:
  dbprod:
    external: true
```

Add a .dockerignore file in .../fullstack/api folder:

```console
touch .dockerignore
```

Add in .dockerignore file '.env' entry to not copy it in container.

../fullstack/api/.dockerignore:

```text
.env
```

Test api with passing credentials at run from command line, from .../fullstack folder:

```console
MONGO_PWD=123 MONGO_USERNAME=paul docker-compose -f docker-compose.prod.yml run api
```

In a second terminal:

```console
docker exec -it fullstack_api_run_16a3f881761e sh

env

NODE_VERSION=16.14.0
HOSTNAME=65742617b66a
YARN_VERSION=1.22.17
SHLVL=1
HOME=/root
TERM=xterm
MONGO_USERNAME=paul
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/app
MONGO_PWD=123
NODE_ENV=production
```

***

## Gitlab, Placing the project on

Browse to [Gitlab](https://gitlab.com/) and create a new blank project called 'docker-production'.

Edit Gitlab user settings to add public SSH Key from local machine, '~/.ssh' folder:

```console
cat id_rsa.pub
```

Copy-paste result to "Add key" form entry in Gitlab.

Back to 'docker-production' project's page, click on clone button and select "Open in your IDE - Visual Studio Code (SSH)" and choose a new local folder (e.g. .../docker-production).

From new VS Code project (docker-production) root folder, in a terminal, copy all files from '.../fullstack' folder:

```console
cp -R ~/.../.../fullstack/* .
```

In .../docker-production folder:

```console
touch .gitignore
```

And following entries to .../docker-production/.gitignore file:

```text
.env
**/node_modules
```

By clicking on 'Source Control' plugin in VS Code, observe that there's no '.env' file in the list.

Finalize git folder initialization:

```console
git config user.email "john@doe.com"
git config user.name "John Doe"
```

First commit:

```console
git add .
git commit -m "first commit"
git push
```

***

## VPS

Setup a Virtual Private Server on [OVHcloud](https://www.ovhcloud.com/fr) or on a Raspberry.

Connect to remote server through SSH:

```console
ssh user@host
```

Create a new user and add it to sudo group and switch to newly created user:

```console
adduser jean
usermod -aG sudo jean
su jean -
```

Back to host machine, set it up to easily connect to remote through SSH:

```console
nano ~/.ssh/config
```

config:

```text
Host dockerprod
    Hostname <ip address>
    User jean
    Port 22
```

Add ssh key to remote server to connect to it without the need to enter the password:

```console
ssh-copy-id jean@<remote server ip address>
```

Now simply connect to remote server with:

```console
ssh dockerprod
```

### Secure configuration of SSH on the server

Connected on remote server through SSH, edit SSH demon configuration for minimal safety:

```console
sudo nano /etc/ssh/sshd_config
```

Edit entries in sshd_config file as follow:

```text
PasswordAuthentication no
X11Forwarding no
PermitRootLogin no
Protocol 2
```

Reload SSH demon:

```console
systemctl reload sshd
```

Now the only way to connect on remote server through SSH is with your own computer with the created user.

Finally change permission on home folder for more safety:

```console
sudo chmod 700 ~
```

### Domain name

Order a domain name.

In DNS Zone, edit type "A" entry with your remote server IP address.

***

## TLS Certificate

Let's Encrypt is a certification authority that allow to get free X509 certificate for using TLS protocol.

They develop ACME protocol to automate certificates management.

Certbot is official client from Let's Encrypt using ACME protocol in an automated manner.

### Certbot Install

Connect to remote server through SSH, then:

```console
sudo snap install certbot --classic
or
sudo apt install certbot
```

Create certificate with certbot, "domain" should exactly match your root URL (with and/or without "www"):

```console
sudo certbot certonly -d DOMAIN1 -d DOMAIN2 -d DOMAIN3
```

Select 'standalone' option, port 80 should be available.
Follow instructions to create a letsencrypt account.

Now, certificates should be available in folder '/etc/letsencrypt/live/DOMAIN_NAME'.

To use TLS certificate edit compose file as follow by opening port 443 for HTTP and HTTP2 for reverse proxy service, docker-compose.prod.yml:

```yaml
version: "3.8"
services: 
  client:
    build:
      context: ./client
      dockerfile: Dockerfile.prod
    restart: unless-stopped
  api:
    build:
      context: ./api
      dockerfile: Dockerfile.prod
    environment: 
      - MONGO_USERNAME
      - MONGO_PWD
      - NODE_ENV=production
    restart: unless-stopped
    depends_on: 
      - db
  db:
    image: mongo
    volumes: 
      - type: volume
        source: dbprod
        target: /data/db
    environment: 
      - MONGO_INITDB_ROOT_USERNAME
      - MONGO_INITDB_ROOT_PASSWORD
    restart: unless-stopped
  reverse-proxy:
    build:
      context: ./reverse-proxy
      dockerfile: Dockerfile.prod
    ports: 
      - 80:80
      - 443:443
    restart: unless-stopped
    depends_on: 
      - api
      - db
      - client
volumes:
  dbprod:
    external: true
```

Edit reverse-proxy/conf/prod.conf to use HTTP2 with nginx:

```text
server {
    listen 80;
    return 301 https://sandbox-dyma.ovh$request_uri;
}

server {
    listen 443 ssl http2;
    ssl_certificate /etc/letsencrypt/live/www.sandbox-dyma.ovh/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/www.sandbox-dyma.ovh/privkey.pem;
    ssl_trusted_certificate /etc/letsencrypt/live/www.sandbox-dyma.ovh/chain.pem;
    ssl_protocols TLSv1.2 TLSv1.3;

    location / {
        proxy_pass http://client;
    }

    location /api {
        proxy_pass http://api;
    }
}
```

We force the redirection of requests on port 80 (HTTP requests) on port 443 (HTTPS / HTTP2).

For the virtual server listening on port 443, we use the certificates created to be able to use HTTP2 (which uses TLS).

To allow reverse proxy to access TLS certificate, create a **bind mount** with modifying docker-compose.prod.yml file as follow:

```yaml
version: "3.8"
services: 
  client:
    build:
      context: ./client
      dockerfile: Dockerfile.prod
    restart: unless-stopped
  api:
    build:
      context: ./api
      dockerfile: Dockerfile.prod
    environment: 
      - MONGO_USERNAME
      - MONGO_PWD
      - NODE_ENV=production
    restart: unless-stopped
    depends_on: 
      - db
  db:
    image: mongo
    volumes: 
      - type: volume
        source: dbprod
        target: /data/db
    environment: 
      - MONGO_INITDB_ROOT_USERNAME
      - MONGO_INITDB_ROOT_PASSWORD
    restart: unless-stopped
  reverse-proxy:
    build:
      context: ./reverse-proxy
      dockerfile: Dockerfile.prod
    ports: 
      - 80:80
      - 443:443
    volumes: 
      - type: bind
        source: /etc/letsencrypt
        target: /etc/letsencrypt
    restart: unless-stopped
    depends_on: 
      - api
      - db
      - client
volumes:
  dbprod:
    external: true
```

We simply mount the /etc/letsencrypt folder which contains our letsencrypt certificates.

***

## Launch production

From local machine.

Git add/commit/push all changes:

```console
git add *
git commit -m "prod ready"
git push origin
```

### Clone from server

Connect to your server through SSH.

Get link (https) to clone from GitLab repository.

Copy paste link to terminal in your server:

```console
git clone https://gitlab.com/...
```

### Docker installation

On Ubuntu server simply type:

```console
sudo snap install docker
```

Check installation:

```console
sudo docker
```

### DB setup

On server, create volume:

```console
sudo docker volume create dbprod
```

Initialize db service:

```console
sudo MONGO_INITDB_ROOT_PASSWORD=password MONGO_INITDB_ROOT_USERNAME=root docker-compose -f docker-compose.prod.yml run -d db
```

Specify user admin root credential, this user is allowed to everything.

Connect to MongoDB client in container:

```console
sudo docker-compose exec -it db mongo
```

Authenticate in console:

```console
use admin
db.auth({user: 'root', pwd: 'password'})
```

Return '1', means it's OK.

Create user used to connect from API to DB:

```console
db.createUser({user: 'jean', pwd: '123', roles:[{role: 'readWrite', db: 'test'}]})
```

Initialize collection in test db:

```console
use test
db.count.insertOne({count: 0});
```

DB is now ready and we may quit:

```console
sudo docker-compose -f docker-compose.prod.yml down
```

### Launch application

We just have to launch our application:

```console
sudo MONGO_USERNAME=jean MONGO_PWD=123 docker-compose -f docker-compose.prod.yml up -d --build
```

We check that everything is well launched:

```console
sudo docker-compose -f docker-compose.prod.yml logs -f
```

You can also access the URL in an Internet browser to test the entire application.

***

## TLS Certificate, automatic renewal

A letsencrypt certificate is valid 90 days.

In this chapter we show you how to automate renewal through a cron job.

### Test renewal command

The pre/post hook are there to free port 80, needed by certbot to renew certificate.

```console
sudo certbot --standalone renew --force-renewal --pre-hook "/snap/bin/docker-compose -f ~/docker-production/docker-compose.prod.yml stop reverseproxy" --post-hook "/snap/bin/docker-compose -f ~/docker-production/docker-compose.prod.yml restart reverseproxy"
```

This command should be adapted to your "/docker-production" folder and check where is installed Docker Compose with 'which docker-compose'.

### Add cron task

We add cron task:

```console
crontab -e
```

Add:

```text
0 0 * * * certbot --standalone renew --pre-hook "/snap/bin/docker-compose -f ~/docker-production/docker-compose.prod.yml stop reverseproxy" --post-hook "/snap/bin/docker-compose -f ~/docker-production/docker-compose.prod.yml restart reverseproxy"
```

We save and we leave the editor: the certificate will be renewed at the right time by certbot automatically. You will have a downtime of a few seconds one day per month approximately every 3 months.

Example code
You can also find the project code on [Github](https://github.com/dymafr/docker-chapitre11-server-prod).

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
