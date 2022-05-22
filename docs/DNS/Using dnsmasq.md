## Install dnsmasq

For Mac OS X

```shell
brew install dnsmasq
```

## Create resolver directory

```shel
$ sudo mkdir /etc/resolver

$ sudo chown YOUR_USERNAME /etc/resolver
```

## Enable Resolver Config for *.dev tld

```shell
echo "nameserver 127.0.0.1" > /etc/resolver/dev
```

## Map all *.dev sub-domains to resolve 127.0.0.1

```shell
echo "address=/.dev/127.0.0.1" >> /usr/local/etc/dnsmasq.conf
```
