# Compose - 9 - Dockerfile

Docker Compose - Dockerfile

!WSL2, advised to not use a mounted volume like '/mnt/c/' for handling a project to avoid slowness and live reload issues.
Instead, prefer usage of a "native" WSL2 folder like for e.g. '/home/user/react-nginx'

Dockerfile and Docker Compose to set up a client application composed of:  
- React  
- NGINX  

***

## Setup of client application project

### Node

'create-React-app' is a script to install with 'npm' will pre-configure a 'webpack' environnement and give use access to a development server and a test server and a build command for production.

Role of 'NGINX' (http server) is to treat all http(s) request and return response from 'React' application to requester.

In this chapter we will see two new features:  
- Dockerfile multi staging, defined with many 'from'.  
- Stdin_open and TTY, two new options of Docker Compose.  

Prerequisite, install on host machine:  
- 'nvm' - [Node Version Manager](https://github.com/nvm-sh/nvm)  
- 'Node.js'

Set/check installation with:
```console

Setup project root folder
$ mkdir react-nginx
$ cd react-nginx

To install 'nvm'
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

Check
$ nvm

Get last version
$ nvm ls-remote | tail
.
.
v17.0.1
        v17.1.0
        v17.2.0
        v17.3.0

Install last version
$ nvm install 17.3.0

$ node -v
v17.3.0

$ node
Welcome to Node.js v17.3.0.       
Type ".help" for more information.
>
(To exit, press Ctrl+C again or Ctrl+D or type .exit)
>
$

```

### React

Instal 'React' with 'npx' (like npm but executed once with last script release):
```console
$ npx create-react-app client
$ cd client
$ ls
README.md  node_modules  package-lock.json  package.json  public  src
$ npm start
.
.
Compiled successfully!

You can now view client in the browser.

  Local:            http://localhost:3000
```

Browse to: - [http://localhost:3000](http://localhost:3000)

Test (launch tests contained in react-nginx/client/src/App.test.js):
```console
$ npm run test
!fail on WSL2 :(
```

Build for production (add a build folder to the project, this is the folder to return via NGINX for client):
```console
$ npm run build
```

All of this just to initialize the project locally on host machine.

### Dockerized

We may now delete the 'node_modules' folder ('node_modules' folder will then be only in container initialized with dependencies through 'npm install' command in 'Dockerfile').

Add a 'Dockerfile' in client project folder '/home/user/react-nginx/client':
```console
$ touch Dockerfile
```

Dockerfile:
```
FROM node:alpine
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["npm", "start"]
```

Build docker image:
```console
$ docker build -t myreact .

$ docker image ls
REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE  
myreact                           latest    c4c1b5bb9d21   45 seconds ago   483MB

$ docker run --rm --name react -p 3000:3000 myreact
```

Browse to: - [http://localhost:3000](http://localhost:3000)

***

## Live reload

For development purpose, to automatically propagate local changes to container.

Bind mount project folder:
```console
$ docker run --rm --name react -p 3000:3000 --mount type=bind,src="$(pwd)",target=/app myreact
```

!! => FAIL!! Why? Because when bind mount it crush all what was contained in container '/app' folder with local content and in local there isn't anymore 'node_modules' folder.

To avoid this unwanted behavior and keep 'node_modules' in container folder not erased by bind mount (also needed for live reload feature), we bind an anonymous volume targeted on remote container '/app/node_modules' folder.

Also, to avoid 'EACCES: permission denied' issue on '/app/node_modules/.cache' folder we modify 'Dockerfile' as follow with giving access to 'node' user:
```
FROM node:alpine
USER node
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["npm", "start"]
```

Bind mount project folder + anonymous volume with '/app/node_modules' as target:
```console
$ docker run --rm --name react -p 3000:3000 --mount type=bind,src="$(pwd)",target=/app --mount type=volume,target=/app/node_modules myreact
```

It's advised to run with '--rm' option when using anonymous volume to suppress it automatically on stop in addition to container suppression.

???

Test live reload by changing text '.. save to reload ..' with e.g. 'Hello, world!" in 'src/App.js' and then observe live effect at [http://localhost:3000](http://localhost:3000) in an Internet browser.

???

***

## Chapter y

### Sub chapter y.1

...

***

## Chapter y

### Sub chapter y.1

...

***


NGINX

React

Node.js

