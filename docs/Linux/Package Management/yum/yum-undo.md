# Yum undo feature

## Reverting back a Yum action

Yum has a "undo" for the transactions.

```shell
yum history list redhat-lsb
```

```shell
Loaded plugins: fastestmirror

ID     | Command line             | Date and time    | Action(s)      | Altered

-------------------------------------------------------------------------------

    33 | erase redhat-lsb         | 2014-11-05 05:20 | Erase          |    1   

    31 | -y install redhat-lsb    | 2014-11-05 05:16 | Install        |  105   
```

```shell
$ yum history undo 31
```

```shell
Loaded plugins: fastestmirror

Undoing transaction 31, from Wed Nov  5 05:16:05 2014

    Dep-Install at-3.1.13-17.el7_0.1.x86_64                       @updates

    Dep-Install avahi-glib-0.6.31-13.el7.x86_64                   @base

    Dep-Install bc-1.06.95-13.el7.x86_64                          @base
```
