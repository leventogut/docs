---
title: yum-cheatsheet
summary: yum-cheatsheet
authors:
    - Levent Ogut
tags:
    - linux
    - yum
    - security
    - package manager
---
## Security updates

### List security updates

```shell
yum updateinfo list updates security
Loaded plugins: extras_suggestions, langpacks, update-motd
amzn2-core/2/x86_64                                                                                             | 3.7 kB  00:00:00     
ALAS2-2022-1807 medium/Sec.    amazon-ssm-agent-3.1.1575.0-1.amzn2.x86_64
ALAS2-2022-1882 medium/Sec.    curl-7.79.1-7.amzn2.0.1.x86_64
ALAS2-2022-1870 important/Sec. dbus-1:1.10.24-7.amzn2.0.2.x86_64
ALAS2-2022-1870 important/Sec. dbus-libs-1:1.10.24-7.amzn2.0.2.x86_64
ALAS2-2022-1874 medium/Sec.    dhclient-12:4.2.5-79.amzn2.1.2.x86_64
...
```

### Install available security updates

```shell
yum -y update --security
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
