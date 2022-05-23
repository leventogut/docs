---
title: Redirect HTTP to HTTPS
summary: Redirect HTTP to HTTPS.
tags:
  - nginx
  - http
  - https
  - redirect
---
```nginx
http-request redirect scheme https unless { ssl_fc }
```
