---
title: Adding HTTP header
summary: Adding HTTP header.
tags:
  - nginx
  - http
  - https
---
```nginx
http-request add-header X-Forwarded-Proto http

http-request add-header X-Forwarded-Proto https

# event better
http-request set-header X-Forwarded-Proto https if { ssl_fc }

http-request set-header X-Forwarded-Proto http if !{ ssl_fc }
```
