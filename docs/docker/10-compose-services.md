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
