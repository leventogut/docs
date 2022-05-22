---
title: Memcached tips
summary: Memcached tips
authors:
    - Levent Ogut
date: 2022-05-22
tags:
    - memcached
    - telnet
---
## Connect to memcached server

Telnet can be used to connect and query Memcached.

```shell
$ telnet localhost 11211
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
get foo
VALUE foo 0 2
```

## Dump cache contents to another instance

```shell
$ memcached-tool 127.0.0.1:11211 dump | nc x.x.x.x 11211
$
```
