---
title: Using dnsmasq
summary: Using dnsmasq.
authors:
    - Levent Ogut
tags:
    - dns
    - dnsmasq
    - local development
---
## Install dnsmasq

??? example

    === "For Debian / Ubuntu"

        ```shell
        sudo apt-get update && apt-get install dnsmasq
        ```

    === "CentOS / RHEL / RockyLinux / AlmaLinux"

        ```shell
        sudo yum install dnsmasq
        ```

    === "For Mac OS X"

        ```shell
        brew install dnsmasq
        ```

## Create resolver directory

```shell
sudo mkdir /etc/resolver
```

```shell
sudo chown $USER:$USER /etc/resolver
```

## Enable resolver configuration for `*.dev` TLD

```shell
echo "nameserver 127.0.0.1" > /etc/resolver/dev
```

## Map all `*.dev` sub-domains to resolve 127.0.0.1

```shell
echo "address=/.dev/127.0.0.1" >> /usr/local/etc/dnsmasq.conf
```

## References and further reading

- [dnsmasq official documentation](https://thekelleys.org.uk/dnsmasq/doc.html)
- [Wikipedia](https://en.wikipedia.org/wiki/Dnsmasq)
- [Man pages](https://linux.die.net/man/8/dnsmasq)
- [Debian Wiki](https://wiki.debian.org/dnsmasq)
