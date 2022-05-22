## Deleting Files Based on Modified Time

```shell
$ find /path/to/files* -mtime +5 -exec rm {} \;
```
