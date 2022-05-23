---
title: Alias match
summary: Alias match
authors:
  - Levent Ogut
tags:
  - httpd
---
```httpd
AliasMatch "^/guides/$" "/var/www/html/index.html"

AliasMatch "^/guides/(.*)/$" "/var/www/html/guides/$1"
```
