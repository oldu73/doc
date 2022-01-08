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
// Setup project root folder
mkdir react-nginx
cd react-nginx

// To install 'nvm'
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

// Check
nvm

// Get last version
nvm ls-remote | tail
.
.
v17.0.1
        v17.1.0
        v17.2.0
        v17.3.0

// Install last version
nvm install 17.3.0

node -v
v17.3.0

node
Welcome to Node.js v17.3.0.       
Type ".help" for more information.
>
(To exit, press Ctrl+C again or Ctrl+D or type .exit)
>
```

### React

Instal 'React' with 'npx' (like npm but executed once with last script release):

```console
npx create-react-app client
cd client
ls
README.md  node_modules  package-lock.json  package.json  public  src
npm start
.
.
Compiled successfully!

You can now view client in the browser.

  Local:            http://localhost:3000
```

Browse to: - [http://localhost:3000](http://localhost:3000)

Test (launch tests contained in react-nginx/client/src/App.test.js):

```console
npm run test
```

!! Fail on WSL2 :(

Build for production (add a build folder to the project, this is the folder to return via NGINX for client):

```console
npm run build
```

All of this just to initialize the project locally on host machine.

### Dockerized

We may now **delete the 'node_modules' folder** ('node_modules' folder will then be only in container initialized with dependencies through 'npm install' command in 'Dockerfile'):

```console
rm -rf node_modules/
```

Add a 'Dockerfile' in client project folder '/home/user/react-nginx/client':

```console
touch Dockerfile
```

Dockerfile:

```text
FROM node:alpine
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["npm", "start"]
```

Build docker image:

```console
docker build -t myreact .

docker image ls
REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE  
myreact                           latest    c4c1b5bb9d21   45 seconds ago   483MB

docker run --rm --name react -p 3000:3000 myreact
```

Browse to: - [http://localhost:3000](http://localhost:3000)

***

## Live reload

It's important to launch **from a terminal** a based VS Code instance from root client application folder to get live reload effect that works:

```console
cd .../react-nginx/client
code .
```

For development purpose, to automatically propagate local changes to container.

Bind mount project folder:

```console
docker run --rm --name react -p 3000:3000 --mount type=bind,src="$(pwd)",target=/app myreact
```

!! => FAIL!! Why? Because when bind mount it crush all what was contained in container '/app' folder with local content and in local there isn't anymore 'node_modules' folder.

To avoid this unwanted behavior and keep 'node_modules' in container folder not erased by bind mount (also needed for live reload feature), we bind an anonymous volume targeted on remote container '/app/node_modules' folder.

Also, to avoid 'EACCES: permission denied' issue on '/app/node_modules/.cache' folder we modify 'Dockerfile' as follow:

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

Bind mount project folder + anonymous volume with '/app/node_modules' as target:

```console
docker run --rm --name react -p 3000:3000 --mount type=bind,src="$(pwd)",target=/app --mount type=volume,target=/app/node_modules myreact
```

It's advised to run with '--rm' option when using anonymous volume to suppress it automatically on stop in addition to container suppression.

Test live reload by changing text '.. save to reload.' with e.g. 'Hello, world!" in 'src/App.js' and then observe live effect at [http://localhost:3000](http://localhost:3000) in an Internet browser.

***

## Set up Docker Compose

docker-compose.yml

```yaml
version: "3.8"
services:
  client:
    build: .
    ports:
      - 3000:3000
    volumes:
      - type: bind
        source: .
        target: /app
      - type: volume
        target: /app/node_modules
```

Reset Docker:

```console
docker system prune -a
docker volume prune
```

Start service:

```console
docker-compose up
```

You may observe live reload working by changing 'App.js' content and 'localhost:3000' changing accordingly.

***

## Test during developpement

Note:

- [webpack](https://webpack.js.org/) is an open-source JavaScript module bundler (wikipedia).  
- TDD (Test Driven Development).

In a devellopement process it's mostly advised to continuously test in order to check that we don't brake anything.  
To achieve this, we lauch two terminals in prallel, one with webpack for running application and this other one for automatic testing purpose.  
It's done with same container image and duplicate services in Docker Compose configuration file, only thing that will change for second duplicated service is that we override command from Dockerfile in Docker Compose configuration file to launch test server and remove also port mapping which is useless for tests.

docker-compose.yml:

```yaml
version: "3.8"
services:
  client:
    build: .
    ports:
      - 3000:3000
    volumes:
      - type: bind
        source: .
        target: /app
      - type: volume
        target: /app/node_modules
  test:
    build: .
    command: ["npm", "run", "test"]
    volumes:
      - type: bind
        source: .
        target: /app
      - type: volume
        target: /app/node_modules
```

```console
docker-compose up --build
```

In logs we may observe:

```console
.
.
test_1    | Tests:       1 passed, 1 total
.
.
```

If we duplicate one exsisting test in '.../src/App.test.js' file, we may observe live change in logs:

```console
.
.
test_1    | Tests:       2 passed, 2 total
.
.
```

To interact with test server we need a second terminal.  
If we want to send command in test container when attaching to it we need to add two options to the test service in Docker Compose configuration file, 'stdin_open' and 'tty', both set to 'true'.

docker-compose.yml:

```yaml
version: "3.8"
services:
  client:
    build: .
    ports:
      - 3000:3000
    volumes:
      - type: bind
        source: .
        target: /app
      - type: volume
        target: /app/node_modules
  test:
    build: .
    command: ["npm", "run", "test"]
    volumes:
      - type: bind
        source: .
        target: /app
      - type: volume
        target: /app/node_modules
    stdin_open: true
    tty: true
```

In first terminal:

```console
docker-compose down -v
docker-compose up --build
```

In second terminal:

```console
docker container ls

CONTAINER ID   IMAGE           COMMAND                  CREATED              STATUS              PORTS                    NAMES
ca16b44236ad   client_test     "docker-entrypoint.s…"   About a minute ago   Up About a minute                            client_test_1
4633b0999f9c   client_client   "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:3000->3000/tcp   client_client_1

docker attach client_test_1
.
.
Watch Usage
 › Press f to run only failed tests.
 › Press o to only run tests related to changed files.
 › Press q to quit watch mode.
 › Press p to filter by a filename regex pattern.
 › Press t to filter by a test name regex pattern.
 › Press Enter to trigger a test run.
```

We may now send commands to test server in second terminal.

Then, shutdown gracefully:

```console
docker-compose down -v
```

***

## Production environnement

### Introduction

NGINX return the build.

NGINX intercept http request and return by default an html file.

Browse [docker hub for nginx on official image page to host some simple static content](https://hub.docker.com/_/nginx#:~:text=Hosting%20some%20simple%20static%20content).

Simple test:

```console
docker run --rm -p 80:80 nginx
```

Browse to [localhost](http://localhost/)

In a second terminal:

```console
docker exec -it exciting_elion sh

cd usr/share/nginx/html

ls
50x.html  index.html

cat index.html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
.
.
```

It's in this folder we gonna copy our application '/usr/share/nginx/html'.

For more complex configuration, have a look in '/etc/nginx/conf.d' folder.

### Multi stage Dockerfile

Reuse a preceding builded image in next build.

Goal to reach here is to put React's build folder into NGINX image.

../client/Dockerfile.prod

```text
FROM node:alpine as buildstage
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
RUN npm run build

FROM nginx
COPY --from=buildstage /app/build /usr/share/nginx/html
EXPOSE 80
```

After second FROM above, --from=buildstage refer to first stage of image build (npm run build, output is a 'build' folder that containe production application).

Build Docker image:

```console
docker build -t nginxreact -f Dockerfile.prod .
.
.
 => [stage-1 2/2] COPY --from=buildstage /app/build /usr/share/nginx/html
.
.

docker image ls
REPOSITORY      TAG       IMAGE ID       CREATED         SIZE
nginxreact      latest    ee3388384541   4 minutes ago   141MB
.
.
<none>          <none>    1c9dc1e2c948   6 hours ago     483MB
.
.
```

"\<none\>" image are produced by intermediate build stage(s).

Launch a container with just built image:

```console
docker run --rm -p 80:80 nginxreact
```

Browse to [localhost](http://localhost/) to observe your production application running.

### Docker Compose

A new Docker Compose configuration file dedicated to prodction:

```console
touch docker-compose.prod.yml
```

docker-compose.prod.yml

```yaml
version: "3.8"
services:
  mynginx:
    build:
      context: .
      dockerfile: Dockerfile.prod
    ports:
      - 80:80
```

Reset Docker content:

```console
docker system prune -a
docker volume prune
```

Start a fresh new production application:

```console
docker-compose -f docker-compose.prod.yml up
```

Browse to [localhost](http://localhost/) to observe your production application running.

***
