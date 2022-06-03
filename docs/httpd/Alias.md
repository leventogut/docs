---
title: Alias match
summary: Alias match
authors:
  - Levent Ogut
tags:
  - httpd
  - apache web server
---
## Alias match

```apache
AliasMatch "^/guides/$" "/var/www/html/index.html"

AliasMatch "^/guides/(.*)/$" "/var/www/html/guides/$1"
```
