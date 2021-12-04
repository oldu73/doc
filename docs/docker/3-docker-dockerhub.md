# Docker - 3 - Docker Hub

***

## export/import

for container
docker container export/import

```
$ docker build -t mynode .

$ docker run -it mynode sh

$c touch hello.txt

$c exit

$ docker container ps -a
CONTAINER ID   IMAGE
3d8e43e502b0   mynode

$ docker container export -o mycontainer.tar 3d8

$ tar -tvf mycontainer.tar

$ docker container rm 3d8

$ docker image import mycontainer.tar nodetest

$ docker images
REPOSITORY                                      TAG
nodetest                                        latest

$ docker image inspect nodetest:latest
```

Only one layer because exporting a container is like creating an image from a file system.

It's not possible to relaunch a container directly from another exported container.
You must first create an image with import.

***

## tar

for image
docker image save/load <-> tar

```
$ docker build -t mynode:0.1 .

$ docker image ls
REPOSITORY                                      TAG       IMAGE ID       CREATED         SIZE  
mynode                                          0.1       c68e7a86d468   8 seconds ago   48MB

$ docker image save -o monimage.tar mynode
```

(to compress with gzip: docker save mon_image | gzip > mon_image.tar.gz)

```
$ tar -tvf monimage.tar

$ docker image prune -a

$ docker image load < monimage.tar
```

or

```
$ docker image load -i mon_image.tar

$ docker run --rm mynode:0.1
hello node-test-012
```

***

## Encrypt identifiers

Encrypt your identifiers GNU/Linux

```
$ sudo apt install pass

$ gpg2 --gen-key
```

Enter your name and email when requested. Then do:

```
$ wget https://github.com/docker/docker-credential-helpers/releases/download/v0.6.3/docker-credential-pass-v0.6.3-amd64.tar.gz && tar -xf docker-credential-pass-v0.6.3-amd64.tar.gz && chmod +x docker-credential-pass && sudo mv docker-credential-pass /usr/local/bin/

$ pass init "YOUR NAME"

$ nano ~/.docker/config.json
```

then:

```
{
    "credsStore": "pass"
}
```

$ docker login

***

## Push image

Push image to docker hub

```
$ docker login

$ docker build -t oldu73/mynode .

$ docker image push oldu73/mynode

$ docker image prune -a

$ docker run --rm oldu73/mynode

$ docker logout
```

***

## Docker hub

docker image pull

docker image push

docker image pull/push <username>/<repertoire>:[tag]

docker search

https://hub.docker.com/

```
$ docker pull node

$ docker image ls
REPOSITORY                                      TAG       IMAGE ID       CREATED         SIZE
node                                            latest    7220633f01cd   7 days ago      992MB

$ docker run -it --rm node sh

$c ls

$c node -v1

$c mkdir app

$c cd app

$c echo "console.log('Hello, world!');" > app.js

$c node app.js
```

***
