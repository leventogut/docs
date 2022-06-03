---
title: Chmod only files or folders
summary: Chmod only files or folders
authors:
    - Levent Ogut
tags:
    - chmod
    - find
    - file
    - directory
---

```shell
cd /my/directory

find . -type d -exec chmod 755 {} \;

find . -type f -exec chmod 644 {} \;

find . -exec chown apache:developers {} \;
```
