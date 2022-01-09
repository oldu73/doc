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
