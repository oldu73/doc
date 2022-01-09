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
