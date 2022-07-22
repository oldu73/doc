# Linux - 02 - Network

***

## Network connection

```console
(sudo) netstat -tunlp
```

***

## Live network connection

Live log established network connections on a dedidcated port number of a load balancer server and forwarded IP of communication server filtered out:

```console
watch -d 'netstat -an | grep :50307 | grep ESTABLISHED | grep -v 10.102.2.26'
```

***
