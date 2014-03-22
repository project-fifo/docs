---
layout: default
category: wiggle
section: wiggle/administration
title: Wiggle Administration
---
# Wiggle Administration
The wiggle admin command is `/opt/local/fifo-wiggle/bin/wiggle-admin`.

## General managemnet
Wiggle uses the SMF to manage it's running state so it is restarted in the case of crashes and booted accordingly on system start. Wiggle can be enabled, disabled and restaerted via: `svcadm enable wiggle`, `svcadm disable wiggle` and `svcadm restart wiggle`

### Updating
Wiggle can be updated by three simple steps.

#### Installing the new package

```bash
pkgin -fy update
pkgin -fy install fifo-wiggle
```

#### Updating the config
After the newest package is installed the config file should be checked for changes and eddited if needed. The `.example` file will always contain the newest version of the config `diff` is a handy tool to see if some settings need to be added to the existing file.

```bash
diff /opt/loca/fifo-wiggle/etc/wiggle.conf /opt/loca/fifo-wiggle/etc/wiggle.conf.example
vi /opt/loca/fifo-wiggle/etc/wiggle.conf
```

#### Restarting the service
After the config is updated the service needs to be restarted, Wiggle is running clustered and has more then `N` it is often possible to do a rolling update by restarting one by one.
