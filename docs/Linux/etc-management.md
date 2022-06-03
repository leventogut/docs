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

## References & further reading

- [etckeeper home](https://etckeeper.branchable.com/)
- [Ubuntu docs](https://ubuntu.com/server/docs/tools-etckeeper)
- [DigitalOcean Tutorial](https://www.digitalocean.com/community/tutorials/how-to-manage-etc-with-version-control-using-etckeeper-on-centos-7)
