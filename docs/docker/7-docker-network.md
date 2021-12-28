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

## Chapter 2

### Sub chapter 2.1

...

***
