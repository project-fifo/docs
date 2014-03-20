---
layout: default
category: wiggle
section: wiggle/administration
title: wiggle administration
---
The wiggle admin command is `/opt/local/fifo-wiggle/bin/wiggle-admin`.


## General managemnet
wiggle uses the SMF to manage it's running state so it is restarted in the case of crashes and booted accordingly on system start. wiggle can be enabled, disabled and restaerted via: `svcadm enable wiggle`, `svcadm disable wiggle` and `svcadm restart wiggle`

### updating
wiggle can be updated by three simple steps.

#### Installing the new package

```bash
pkgin -fy update
pkgin -fy install fifo-wiggle
```

#### updating the config
After the newest package is installed the config file should be checked for changes and eddited if needed. The `.example` file will always contain the newest version of the config `diff` is a handy tool to see if some settings need to be added to the existing file.

```bash
diff /opt/loca/fifo-wiggle/etc/wiggle.conf /opt/loca/fifo-wiggle/etc/wiggle.conf.example
vi /opt/loca/fifo-wiggle/etc/wiggle.conf
```

#### restarting the service
After the config is updated the service needs to be restarted, wiggle is running clustered and has more then `N` it is often possible to do a rolling update by restarting one by one.
