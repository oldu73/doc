# Compose - 10 - Services

Docker Compose - Services

Use of Docker Compose with many services.

In this chapter we setup a complete development/production environnement close to what we may found in real life projects, sclable and so on.

***

## Project architecture

Introduction to project architecture:

- React (Application), could be Vue.js or Angular, would be the same.  
- NGINX (Http request).  
- Node.js (API).  
- MongoDB (database).

### Developpement

NGINX as reverse proxy follow requests:

- != /api + == /sockjs-node (websocket for live reload feature) -> React.  
- == /api -> Node.js -> MongoDB.

### Production

NGINX as reverse proxy (on big application may also be used as a load balancer) follow requests:

- != /api -> another NGINX that handle React production build output ('build' folder).  
- == /api -> Node.js -> MongoDB.

### Setup

```console
mkdir fullstack
cd fullstack
```

React applicatiion:

```console
mkdir client
```

Node.js applicatiion:

```console
mkdir api
```

MongoDB database:

```console
mkdir db
```

NGINX as a reverse proxy, in charge of dispatching http requests:

```console
mkdir reverse-proxy
```

Docker Compose:

```console
touch docker-compose.dev.yml
touch docker-compose.prod.yml
```

***

## Client configuration

Setting up the client configuration

```console
cd client
npx create-react-app client
mv client/* .
rm -rf client
rm -rf node_modules
touch Dockerfile.dev
```

/fullstack/client/Dockerfile.dev:

```text
FROM node:alpine
WORKDIR /app
COPY package.json .
RUN npm install
# To avoid 'EACCES: permission denied' issue on '/app/node_modules/.cache' folder
RUN mkdir -p node_modules/.cache && chmod -R 777 node_modules/.cache
COPY . .
CMD ["npm", "start"]
```

/fullstack/docker-compose.dev.yml:

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
```

From 'fullstack' folder, we'll let '--build' option to ensure building images before starting containers:

```console
code .
docker-compose -f docker-compose.dev.yml up --build
```

Test by browsing at [localhost:3000](http://localhost:3000/)

We add a function to '/fullstack/client/src/App.js' file in order to get a counter from api (not ready yet but we prepare here the application in advance).

/fullstack/client/src/App.js:

```javascript
import logo from './logo.svg';
import './App.css';
import { useState, useEffect } from 'react';

function App() {
  const [count, setCount] = useState();

  useEffect(() => {
    async function fetchCount() {
      try {
        const response = await fetch('/api/count')
        if (response.ok) {
          setCount(await response.json());
        }
      } catch (e) {
        console.log(e);
      }
    }
    fetchCount();
  }, []);

  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <p>
          Count : { count }
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;
```

In Docker Compose logs we may observe succesfull compilation:

```console
.
.
client_1  | webpack 5.65.0 compiled successfully in 191 ms
```

Test again by browsing at [localhost:3000](http://localhost:3000/)

Stop/remove stack:

```console
docker-compose -f docker-compose.dev.yml down
```

Note: - webpack is exclusively devoted for development.

***

## Set up Node.js API

From project root folder: - .../fullstack

```console
cd api
npm init -y
npm i express nodemon mongodb
mkdir src
cd src
touch index.js
```

index.js (from previous node server project)

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
```

.../fullstack/docker-compose.dev.yml

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
      dockerfile: Dockerfile
    volumes:
      - type: bind
        source: ./api/src
        target: /app/src
    ports:
      - 3001:80
```

../fullstack/api

```console
touch Dockerfile
```

.../fullstack/api/Dockerfile

```text
FROM node:alpine
WORKDIR /app
COPY package.json .
RUN npm install
# To avoid 'EACCES: permission denied' issue on '/app/node_modules/.cache' folder
RUN mkdir -p node_modules/.cache && chmod -R 777 node_modules/.cache
COPY . .
EXPOSE 80
CMD ["npm", "start"]
```

.../fullstack/api/package.json

```json
{
  "name": "api",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "nodemon ./src/index.js",
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

```

Test API in a terminal from .../fullstack/ folder

```console
code .
docker-compose -f docker-compose.dev.yml run api
```

***

## Set up database service

Add db service to Docker Compose configuration file.

.../fullstack/docker-compose.dev.yml:

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
      dockerfile: Dockerfile
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
volumes:
  dbtest:
```

Initialize database container.

From '.../fullstack' folder:

```console
docker-compose -f docker-compose.dev.yml run db
```

Now it should be launch and we connect on it in a second treminal:

```console
docker container exec -it fullstack_db_run_c97d6cfbd602 sh
mongo
use test
db.count.insertOne({ count: 0 })
exit
exit
```

Now it's OK, we may close second terminal and 'Ctrl+c' database container.

We may test application with (from '.../fullstack' folder):

```console
(code . // it's always better to have VS Code started from application root folder)
docker-compose -f docker-compose.dev.yml up
```

Test application by broswsing to [http://localhost:3001/api/count](http://localhost:3001/api/count)

We may also observe that React is running by browsing to [http://localhost:3000/](http://localhost:3000/)

Then 'Ctrl-c' to stop stack.

***

## Set up the NGINX reverse proxy

From '.../fullstack/reverse-proxy' folder:

```console
mkdir conf
touch conf/dev.conf
touch Dockerfile.dev
```

.../fullstack/reverse-proxy/Dockerfile.dev:

```text
FROM nginx:latest
COPY ./conf/dev.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
```

.../fullstack/reverse-proxy/conf/dev.conf:

```text
server {
    listen 80;

    location / {
        proxy_pass http://client:3000;
    }

    location /api {
        proxy_pass http://api;
    }

    location /sockjs-node {
        proxy_pass http://client:3000;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

No need to specify port for api because inside stack (network), not from a host point of view, it's listenning on default http port ('80').

Among HTTP standards, by default some 'headers' called 'hop-by-hop' aren't passed by proxy to server and then avoid live reload to work.

To fix this, the 'sockjs-node' is a needed technical part to make live reload works.

.../fullstack/docker-compose.dev.yml:

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
      dockerfile: Dockerfile
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

Test, everything is supposed to work:

```console
docker-compose -f docker-compose.dev.yml up --build
```

In a second terminal:

```console
docker-compose -f docker-compose.dev.yml ps
NAME                        COMMAND                  SERVICE             STATUS              PORTS
fullstack_api_1             "docker-entrypoint.s…"   api                 running             0.0.0.0:3001->80/tcp
fullstack_client_1          "docker-entrypoint.s…"   client              running             0.0.0.0:3000->3000/tcp
fullstack_db_1              "docker-entrypoint.s…"   db                  running             27017/tcp
fullstack_reverse-proxy_1   "/docker-entrypoint.…"   reverse-proxy       running             0.0.0.0:80->80/tcp
```

We may obeserve then the 4 components composing our application are running.

And test by browsing to [http://localhost/](http://localhost/), note that there's not need to specify port this time thanks to NGINX that do his job as a reverse proxy by distributing requests through our application by listening on default http port ('80').

By refreshing the page, you also may observe counter increasing.

To test live reload edit file '.../fullstack/client/src/App.js' and observe live changes in browser window (even without refreshing the page).

We now have a full functionnal development stack that maybe up with only one command:

```console
docker-compose -f docker-compose.dev.yml up
```

***

## Set up production configuration

Reset Docker environnemnt (if needed):

```console
docker system prune -a
docker volume prune
```

### Client

From folder '.../fullstack/client':

```console
touch Dockerfile.prod
```

.../fullstack/client/Dockerfile.prod

```text
FROM node:alpine as build
WORKDIR /app
COPY package.json .
RUN npm install
# To avoid 'EACCES: permission denied' issue on '/app/node_modules/.cache' folder
RUN mkdir -p node_modules/.cache && chmod -R 777 node_modules/.cache
COPY . .
RUN npm run build

FROM nginx:latest
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
```

! Be aware of **RUN** npm run build, NOT CMD!

.../fullstack/docker-compose.prod.yml

```yaml
version: '3.8'
services:
  client:
    build:
      context: ./client
      dockerfile: Dockerfile.prod
    restart: unless-stopped
```

Start VS Code in '.../fullstack' folder with 'code .' command and then:

```console
docker-compose -f docker-compose.prod.yml run -p 80:80 client
```

Browse to [http://localhost/](http://localhost/) to validate React application is running.

### API

In '.../fullstack/api/src/index.js' notice mongodb connection URL with MONGO_USERNAME and MONGO_PWD credentials that comes from process (production) environnement:

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
```

In '.../fullstack/api' folder:

```console
touch .env
```

.../fullstack/api/.env:

```text
MONGO_USERNAME=paul
MONGO_PWD=123
```

We don't want that secret informations to be copied in container:

```console
touch .dockerignore
```

.../fullstack/api/.dockerignore:

```text
.env
```

.../fullstack/docker-compose.prod.yml

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
      dockerfile: Dockerfile
    env_file:
      - ./api/.env
    environment:
      NODE_ENV: production
    restart: unless-stopped
```

Test API with (tanks to "console.log(process.env)" in ".../fullstack/api/src/index.js" we may observe environment variables):

```console
docker-compose -f docker-compose.prod.yml run api
{
  .
  .
  .
  MONGO_USERNAME: 'paul',
  .
  .
  MONGO_PWD: '123',
  .
  NODE_ENV: 'production',
  .
  .
}
```

Hit 'Ctrl+c' to stop.

### DB

Secure the database by providing root username and password through an environment file.

From folder '.../fullstack/db':

```console
touch .env
```

Get credential synthax from 'Environment Variables' section in [Mongo image on Docker Hub](https://hub.docker.com/_/mongo).

In file '.../fullstack/db/.env' add credentials:

```text
MONGO_INITDB_ROOT_USERNAME=admin
MONGO_INITDB_ROOT_PASSWORD=password
```

Provide environment file (for root user credentials) and external volume for db service in docker compose configuration file.

Set up db service in '.../fullstack/docker-compose.prod.yml' file:

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
      dockerfile: Dockerfile
    env_file:
      - ./api/.env
    environment:
      NODE_ENV: production
    restart: unless-stopped
  db:
    image: mongo
    volumes:
      - type: volume
        source: dbprod
        target: /data/db
    env_file:
      - ./db/.env
    restart: unless-stopped

volumes:
  dbprod:
    external: true
```

Create volume 'dbprod':

```console
docker volume create dbprod
```

Run 'db' container in detached mode from '.../fullstack' folder:

```console
docker-compose -f docker-compose.prod.yml run -d db
```

Initialize database by connecting to it with root user credentials.

```console
docker container exec -it fullstack_db_run_e9fa9328e2b8 sh
mongo
use admin
db.auth({ user: 'admin', pwd: 'password' })
```

Create the needed user for API access, the one described in '.env' envrionement file that reside in '.../fullstack/api' folder.

```console
db.createUser({ user: 'paul', pwd: '123', roles: [{ role: 'readWrite', db: 'test' }] })
```

Setup collection for 'count'.

```console
use test
db.count.insertOne({ count: 0 })
exit
exit
docker stop fullstack_db_run_e9fa9328e2b8
```

Now db is ready and API may communicate with it.

To test, lauch complete stack with port 80 open for 'api' service (only for testing, because after, in this production context, this is the reverse proxy role to communicate on port 80).

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
      dockerfile: Dockerfile
    env_file:
      - ./api/.env
    environment:
      NODE_ENV: production
    restart: unless-stopped
    ports:
      - 80:80
  db:
    image: mongo
    volumes:
      - type: volume
        source: dbprod
        target: /data/db
    env_file:
      - ./db/.env
    restart: unless-stopped

volumes:
  dbprod:
    external: true
```

Launch full stack:

```console
docker-compose -f docker-compose.prod.yml up (--build)
```

Browse to [localhost/api/count](http://localhost/api/count) and refresh page to observe counter incrementing.

Hit 'Ctrl+c' to stop.

Remove port 80 for 'api' service in 'docker-compose.prod.yml' file.

### Reverse proxy

Production configuration file in '.../fullstack/reverse-proxy/conf' folder:

```console
touch prod.conf
```

Production Docker file in '.../fullstack/reverse-proxy' folder:

```console
touch Dockerfile.prod
```

.../fullstack/reverse-proxy/Dockerfile.prod:

```text
FROM nginx:latest
COPY ./conf/prod.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
```

.../fullstack/reverse-proxy/conf/prod.conf:

```text
server {
    listen 80;

    location / {
        proxy_pass http://client;
    }
    
    location /api {
        proxy_pass http://api;
    }
}
```

Add 'reverse-proxy' service in '.../fullstack/docker-compose.prod.yml' file:

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
      dockerfile: Dockerfile
    env_file:
      - ./api/.env
    environment:
      NODE_ENV: production
    restart: unless-stopped
    depends_on:
      - db
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

Test, from '.../fullstack' folder:

```console
docker-compose -f docker-compose.prod.yml down -v
docker-compose -f docker-compose.prod.yml up --build
```

Browse to [localhost](http://localhost) and refresh page to observe counter incrementing.

***
