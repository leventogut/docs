---
title: yum-cheatsheet
summary: yum-cheatsheet
authors:
    - Levent Ogut
tags:
    - linux
    - yum
    - security
---
## Security updates

```shell
yum updateinfo list updates security
```

## Update except

```shell
yum update --exclude=mysql*
```


```shell
# /etc/yum.conf
[main]
...
exclude=mysql* php*
```

## Dependency list of a package

```shell
yum deplist sqlite
```

## Which package provides /directory/file

```shell
yum provides /usr/sbin/sw-engine-fpm
```
