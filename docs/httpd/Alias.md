
```httpd
AliasMatch "^/guides/$" "/var/www/html/index.html"

AliasMatch "^/guides/(.*)/$" "/var/www/html/guides/$1"
```
