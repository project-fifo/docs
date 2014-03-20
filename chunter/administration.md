---
layout: default
section: chunter
section: chunter/administration
title: Chunter administration
---

## General managemnet
chunter uses the SMF to manage it's running state so it is restarted in the case of crashes and booted accordingly on system start. chunter can be enabled, disabled and restaerted via: `svcadm enable chunter`, `svcadm disable chunter` and `svcadm restart chunter`

### updating
Chunter can be updated by running `/opt/chunter/bin/update` this script will check for new updates and install them as needed.

#### Installing the new package

```bash
/opt/chunter/bin/update
```

#### updating the config
After the newest package is installed the config file should be checked for changes and eddited if needed. The `.example` file will always contain the newest version of the config `diff` is a handy tool to see if some settings need to be added to the existing file.

```bash
diff /opt/chunter/etc/chunter.conf /opt/chunter/etc/chunter.conf.example
vi /opt/chunter/etc/chunter.conf
```

#### restarting the service
Chunter cna be restarte by running `svcadm restart chunter`.

### Manually created VM's
Chunter does not automatically detect manually created VM's to make chunter aware of them chunter needs to be restarted: `svcadm restart chunter`.
