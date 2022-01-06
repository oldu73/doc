# Compose - 9 - Dockerfile

Docker Compose - Dockerfile

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
!really slow on WSL2 :(
.
.
Compiled successfully!

You can now view client in the browser.

  Local:            http://localhost:3000
```

Browse to: - [http://localhost:3000](http://localhost:3000)

WIP pointer: (video - 06:33)



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

