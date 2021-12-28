# Docker - 7 - Network

***

## Introduction

WAN = Internet  
LAN = Local

docker network
- ls
- create
- rm
- inspect
- connect
- disconnect
- prune
- \--network \| \--net

3 methods:
- Bridge, sub-segment
- Host, merge host machine network
- Overlay (Docker Swarm) 
- (MACVLAN)
- (Others)

### Bridge (mainly used)

Grouped by sub-segment.  
Docker as bridge manager.  
By default a container belongs to named "bridge" (Docker0) network.

### Host (Linux only, rarely used)

IP addresses for containers defined by router like for host machine.
Containers will be straight forward connected to local network.

### Overlay (Swarm)

To establish communication between Docker Daemons.

***

## Bridge
```console
$ docker network
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
266b7ae5e9d7   bridge    bridge    local
ada5f50a5c41   host      host      local
119b5f46e464   none      null      local

$ docker network inspect bridge
...
"Containers": {},
...

$ ifconfig
...
docker0:
...

$ docker run --rm -it alpine sh
```

in a second terminal
```console
$ docker network inspect bridge
...
"Containers": {
            "04d3d4540a17d21ea7db83779e8de1716e6e3a4122e1f2c2f66c60d25a094656": {
                "Name": "stoic_wozniak",
                "EndpointID": "2ed11c55d850ed3cc4eec221f705dad2a9679a016934cd991cd96da86d2dfcbd",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
}
...

$ docker run --rm -it alpine sh
```

in a third terminal
```console
$ docker network inspect bridge
"Containers": {
            "04d3d4540a17d21ea7db83779e8de1716e6e3a4122e1f2c2f66c60d25a094656": {  
                "Name": "stoic_wozniak",
                "EndpointID": "2ed11c55d850ed3cc4eec221f705dad2a9679a016934cd991cd96da86d2dfcbd",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            },
            "76ff8f56718ae5244eabe03092f7a0227aa2e42249bf2ab1c8f91a9faf76c715": {  
                "Name": "admiring_driscoll",
                "EndpointID": "684e6a570db46dadd9bdf53bf383ea069fa626fc16b7ab26c11e604415c00b25",
                "MacAddress": "02:42:ac:11:00:03",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            }
}
```

in second terminal
```console
$c ping google.ch
$c ping 172.17.0.2
```

Be aware that ip address maybe attributed randomly by Docker.  
To use name on default network, use **--name** and **--link** (deprecated) options on run command then you may ping by name instead of ip address (only for default bridge network).

in first terminal
```console
$ docker run --rm --name alpine1 -it alpine sh
```

in second terminal
```console
$ docker run --rm --link alpine1 -it alpine sh
$c ping alpine1
```

***

## Chapter y

### Sub chapter y.1

...

***
