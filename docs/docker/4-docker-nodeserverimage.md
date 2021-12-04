# Docker - 4 - Node Server Image

***

## Project server node

Project server node to return a minimal page in a browser at localhost address,
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
