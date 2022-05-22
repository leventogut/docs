## Signals

The master process supports the following signals:

```other
TERM, INT       fast shutdown
QUIT            graceful shutdown
HUP             changing configuration, keeping up with a changed time zone (only for FreeBSD and Linux), starting new worker processes with a new configuration, graceful shutdown of old worker processes
USR1            re-opening log files
USR2            upgrading an executable file
WINCH           graceful shutdown of worker processes
```

Containers running Nginx as main process should include following to their Dockerfile to enable graceful shutdown.

```Dockerfile
STOPSIGNAL SIGQUIT
```

## References

- [Controlling Nginx](http://nginx.org/en/docs/control.html)
