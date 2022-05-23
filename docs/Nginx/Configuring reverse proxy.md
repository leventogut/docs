---
title: Configuring reverse proxy
summary: Configuring reverse proxy
authors:
  - Levent Ogut
tags:
  - nginx
---
```nginx
upstream appA {
        server 127.0.0.1:3000;
}

upstream appB {
        server 127.0.0.1:4000;
}

server {
  listen 80;
  server_name appa.domain.com;
  
  location / {
        proxy_pass http://appA;
    }
}

server {
  listen 80;
  server_name appb.domain.com;

  location / {
        proxy_pass http://appB;
    }
}
```
