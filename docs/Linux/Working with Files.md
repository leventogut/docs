## Deleting Files Based on Modified Time

```shell
find /path/to/files* -mtime +5 -exec rm {} \;
```

## chmod 755 only directories

```shell
find . -type d -exec chmod 755 {} \;
```

## chmod 644 only files

```shell
find . -type f -exec chmod 644 {} \;
```
