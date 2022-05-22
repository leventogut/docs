# Creating self signed certificate using openssl

> (CN should be *.domain.com for wildcard certs)

## Creating certificate signing request (CSR)

```shell
export DOMAIN=example.com
export COUNTRY=US
export STATE="New York"
export CITY="New York City"
export ORGANIZATION="MyCompany Inc."
$ openssl req -new -newkey rsa:2048 -nodes -out $DOMAIN.csr -keyout $DOMAIN.key -subj "/C=$COUNTRY/ST=$STATE/L=$CITY/O=$ORGANIZATION/CN=$DOMAIN"
```

### Verifying the generated CSR

```shell
$ openssl req -text -noout -verify -in $DOMAIN.csr
```

### Creating the certificate

```shell
$ openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout $DOMAIN.key -out $DOMAIN.crt
```

### Testing the certificate

```shell
$ openssl s_client -connect x.x.x.x:443 -servername $DOMAIN
openssl s_client -connect 139.178.84.217:443 -servername kernel.org
CONNECTED(00000005)
depth=2 C = US, O = Internet Security Research Group, CN = ISRG Root X1
verify return:1
depth=1 C = US, O = Let's Encrypt, CN = R3
verify return:1
depth=0 CN = dfw.source.kernel.org
verify return:1
---
Certificate chain
 0 s:/CN=dfw.source.kernel.org
   i:/C=US/O=Let's Encrypt/CN=R3
 1 s:/C=US/O=Let's Encrypt/CN=R3
   i:/C=US/O=Internet Security Research Group/CN=ISRG Root X1
 2 s:/C=US/O=Internet Security Research Group/CN=ISRG Root X1
   i:/O=Digital Signature Trust Co./CN=DST Root CA X3
---
...
```

## References

- [Most Common OpenSSL Commands](https://www.sslshopper.com/article-most-common-openssl-commands.html)
- [A good explanation of SSL Certificate Formats](http://serverfault.com/questions/9708/what-is-a-pem-file-and-how-does-it-differ-from-other-openssl-generated-key-file)
- [OpenSSL Quick Reference Guide](https://www.digicert.com/kb/ssl-support/openssl-quick-reference-guide.htm)
