---
title: SetGID practical example
summary: SetGID practical example
authors:
    - Levent Ogut
tags:
    - linux
    - setgid
    - chmod
---
## Requirements

We want webserver root to be writable by users who has developers group.
New files should inherit `chown :apache` and `chmod 664` so that all developers can read and write.

## Create `developers` group and a sample user

```shell
groupadd developers

useradd -g developers --create-home developer1 \
        --key PASS_MAX_DAYS=60 \
        --key PASS_MIN_DAYS=0 \
        --key PASS_MIN_LEN=14 \
        --key PASS_WARN_AGE=7
```

## Modify sudoers

```shell
%developers ALL=(ALL) NOPASSWD: ALL, !DISABLE_SU
```

## Verifying the user

```shell
id developer1
uid=1004(developer1) gid=1005(developers) groups=1005(developers)

chage -l developer1
Last password change                                    : Jun 03, 2022
Password expires                                        : Aug 02, 2022
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 60
Number of days of warning before password expires       : 7
```


## Change ownership of the files

```shell
find /var/www/html -exec chown apache:apache {} \;
```

## Enabling SetGID

```shell
chmod g+s /var/www/html # chmod 2775 /var/www/html
```

## Chmod 775 only directories

```shell
find /var/www/html -type d -exec chmod 775 {} \;
```

## Chmod 664 only files

```shell
find /var/www/html -type f -exec chmod 664 {} \;
```
