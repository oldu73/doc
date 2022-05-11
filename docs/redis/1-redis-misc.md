# Redis - 01 - Misc

***

## Basic operations (CRUD)

Basic operations from command line interface ([source](https://auth0.com/blog/introduction-to-redis-install-cli-commands-and-data-types/)).

### Test

```console
redis-cli ping
```

### Client

```console
redis-cli
```

### Create

```console
SET mykey "Hi!"
```

### Read

```console
GET mykey
```

### Update

```console
SET mykey "Hello"
```

### Delete

```console
DEL mykey
```

### List all keys

```console
keys *
```

### Monitor

[Source](https://redis.io/commands/MONITOR)

```console
redis-cli monitor
```

### Quit

```console
exit
```

***

### Remove all

To remove all the keys of all the existing database, run:

```console
redis-cli FLUSHALL
```

***
