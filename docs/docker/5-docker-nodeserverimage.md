# Docker - 5 - Node Server Image

***

## Stats

Show resource usage statistic

Maybe tried with Node server project running

```
$ docker run --rm -d --name appnode -p 80:80 myapp
```

Then

```
$ docker stats
```

After, open a browser at http://localhost/ address

Click a lot on refresh button to see CPU % growing in stats terminal's live view.

***

## Detach mode

-d option

```
$ docker run --rm -d --name appnode -p 80:80 myapp
```

--rm to remove container automatically after stop

Then to go back inside running detached container

```
$ docker exec -it appnode sh
```

***

## .dockerignore

In current project folder, create a new file named ".dockerignore"

Add also a sample text file "hello.txt" file that maybe contain "to be ignored" sentence.

Re-build image

```
$ docker build -t myapp .
```

Launch it in interactive mode with sh

```
$ docker run --rm -it myapp sh
```

ls/cat hello.txt file in container output respectively presence/content of hello.txt file.

Now list hello.txt file in .dockerignore file

```
# for current folder
hello.txt

# for first level of folder
*/hello.txt

# for everywhere
**/hello.txt

# exception with !
**/*.txt
!README.txt
```

Re-build image

Re-launch it in interactive mode

ls output does not contain hello.txt file anymore

***

## Optimization

Let's say we change listening port of Node server project from 80 to 70

file app.js

```
const express = require('express');

const app = express();

app.get('*', (req, res) => {
    res.status(200).json('Hello, world!');
})

app.listen(70);

```

Re-build image

```
$ docker build -t myapp .

=> [4/4] RUN npm install 7.1s
```

What we observe here is that NPM install is run again although only listening port in app.js has been modified.

To avoid this behavior, Docker file should be modified accordingly.

Split COPY instruction in two, before and after "RUN npm install" to not invalidate cache for dependencies (package.json).

Dockerfile

```
FROM node:alpine

WORKDIR /app

COPY ./package.json .

RUN npm install

COPY . .

ENV PATH=$PATH:/app/node_modules/.bin

CMD ["nodemon", "app.js"]
```

And now, on build image after changing listening port in app.js

```
$ docker build -t myapp .

=> [4/4] RUN npm install 0.0s
```

***

## Redirect port

Redirect port from host to container

```
docker run ... -p <hostport>:<containerport> ...
```

-p option:

-p, --publish list Publish a container's port(s) to the host

To fix Node server project, add missing port redirection to run command:

```
$ docker run --rm -p 80:80 myapp
```

Many containers may listen on same port number because they're isolated.

```
$ docker run --rm -d -p 81:80 myapp

$ docker run --rm -d -p 82:80 myapp
```

On the other hand, on the host, only one forwarding on a port is possible. One port = one application!

Without opening port, a container may access to Internet.

Outgoing traffic is possible.

By default, all incoming traffic is blocked, all ports are closed by default.

***

## Port

http 80

https 443

ssh 22

***

## Node server project

Node server project to return a minimal page in a browser at localhost address,
with Express (which is a Node.js framework)
  
Our image should have Node.js and its npm package manager.
It should have several dependencies: nodemon and express.
And it will have to launch the app contained in app.js by default.

For the project, we'll create a new Dockerfile in a folder.

In the same folder we will also have a package.json file (which allows you to manage the dependencies used) and an app.js file (which will contain our application).
In the package.json file therefore have our two dependencies.

In the app.js file we just have an Express route which will send in JSON format the character string "Hello, world!" for all routes.

folder: - /mnt/c/tmp/docker/node-server

```
$ touch package.json
edit package.json

{
    "dependencies": {
        "nodemon": "2.0.14",
        "express": "4.17.1"
    }
}
```

```
$ touch app.js
edit app.js

const express = require('express');

const app = express();

app.get('*', (req, res) => {
    res.status(200).json('Hello, world!');
})

app.listen(80);
```

```
$ touch Dockerfile
```

Browse a bit docker hub and look for alpine tagged image more lighter (169MB) than the official one (latest) (992MB)

```
FROM node:alpine

WORKDIR /app

COPY . .

RUN npm install

CMD ["nodemon", "app.js"]
```

then..

```
$ docker build -t myapp .

$ docker image ls

$ docker run --rm myapp
```

path error

to debug, run in interactive mode with sh to override Dockerfile CMD

```
$ docker run --rm -it myapp sh

/app $c env

/app $c nodemon
sh: nodemon: not found

$c cd node_modules/.bin/
/app/node_modules/.bin # ls
is-ci      mime       nodemon

/app $c ./node_modules/.bin/nodemon app.js
```

Fix the Docker file with adding value to image's path

```
..

ENV PATH=$PATH:/app/node_modules/.bin
..
```

```
$ docker build -t myapp .

$ docker run --rm -it myapp sh
/app $c env | grep PATH
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/app/node_modules/.bin

/app $c nodemon app.js
```

Now it's OK

```
$ docker run --rm myapp
```

Try "localhost" in a browser -> does not work and it's normal because default behavior does not allow
to communicate with a container.

***
