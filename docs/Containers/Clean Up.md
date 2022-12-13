---
title: Container image clean up
summary: Container image clean up
authors:
    - Levent Ogut
tags:
    - container
    - docker
    - podman
---

## Removing images

```shell
$ docker rmi [image id | image name]
$

$ alias drmi="docker rmi -f $(docker images | grep "<none>" | awk "{print \$3}")"
$
```

## Removing dangling, stopped containers and all unused images

```shell
$ docker system prune -a
$
```
