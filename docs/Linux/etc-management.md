---
title: Putting /etc under version control
summary: Putting /etc under version control
authors:
    - Levent Ogut
tags:
    - linux
    - etc
    - etckeeper
---
## Putting /etc under version control

```shell
cd /etc/
etckeeper init
etckeeper commit "inital commit"
```

You can also add a remote origin to backup/sync.
