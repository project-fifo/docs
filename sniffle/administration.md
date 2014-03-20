---
layout: default
category: sniffle
section: sniffle/administration
title: Sniffle administration
---
The sniffle admin command is `/opt/local/fifo-sniffle/bin/sniffle-admin` but many commands can also be accessed via `fifoadm` command. Please keep in mind that `fifoadm` is not designed as a every day command but only as a last fallback when commands are not available through the API.


## General managemnet
Sniffle uses the SMF to manage it's running state so it is restarted in the case of crashes and booted accordingly on system start. Sniffle can be enabled, disabled and restaerted via: `svcadm enable sniffle`, `svcadm disable sniffle` and `svcadm restart sniffle`

### updating
Sniffle can be updated by three simple steps.

#### Installing the new package

```bash
pkgin -fy update
pkgin -fy install fifo-sniffle
```

#### updating the config
After the newest package is installed the config file should be checked for changes and eddited if needed. The `.example` file will always contain the newest version of the config `diff` is a handy tool to see if some settings need to be added to the existing file.

```bash
diff /opt/loca/fifo-sniffle/etc/sniffle.conf /opt/loca/fifo-sniffle/etc/sniffle.conf.example
vi /opt/loca/fifo-sniffle/etc/sniffle.conf
```

#### restarting the service
After the config is updated the service needs to be restarted, sniffle is running clustered and has more then `N` it is often possible to do a rolling update by restarting one by one.

## Cluster management

### sniffle-admin join `<nodename>@<ip>`
Joins a sniffle cluster, please note that all data on the joining (not the joined) node is deleted.

### sniffle-admin leave
Cleanly removes a node from the ring. This is helpful when nodes get moved and the cluster downsized

### sniffle-admin remove `<nodename>@<ip>`
Forcefully removes a node from the ring. This can be used after fatal node cluster.

### sniffle-admin member-status
Lists the status of each node and the distribution of data over the ring nodes.

```
================================= Membership ==================================
Status     Ring    Pending    Node
-------------------------------------------------------------------------------
valid     100.0%      --      'snarl@172.16.0.254'
-------------------------------------------------------------------------------
Valid:1 / Leaving:0 / Exiting:0 / Joining:0 / Down:0
```

### sniffle-admin ring-status
Gives a extended report ont he ring, including handoffs and downed nodes.

```
================================== Claimant ===================================
Claimant:  'snarl@172.16.0.254'
Status:     up
Ring Ready: true

============================== Ownership Handoff ==============================
No pending changes.

============================== Unreachable Nodes ==============================
All nodes are up and reachable
```

### sniffle-admin status
A simple command that returns the overall cluster status, it returns a propper return code and is useful for scripted rolling updates.


### sniffle-admin aae-status

Gives a detailed status on the AAE status of the system.

## Log Files
FiFo uses extensive logging to make debugging issue and understanding behaviour. The log files are located in `/var/log/sniffle/`. There are multiple log files with various severities.


### debug.log
By default the debug log is disabled, it is very verbose and should not be enabled in production systems to enable it uncomment the followig line in the `sniffle.conf`

```
log.debug.file = /var/log/sniffle/debug.log
```

### console.log
This file contains logs of the level info and above, usually all interesting logs can be found here.

### error.log
This files contains errors, it usually should be mostly empty but please keep in mind that failing is not a uncommon practice to deal with unexpected behavuiour so sporadic entries might just be fine.



## Global configuration
In addition to the config files that apply on a per node level there are global configurations that can be changed from one system and are applied globally. Unless otherwise noted all those settings can be changed during runtime.

### sniffle-admin config show
Shows a list of all settings in the global configuration.

```
  General Section
                 Key                                              Value
-------------------- --------------------------------------------------
storage.general.back internal
  S3 Section
                 Key                                              Value
-------------------- --------------------------------------------------
storage.s3.general_b                                               fifo
     storage.s3.port 8443
storage.s3.access_ke                                                ...
storage.s3.snapshot_                                     fifo-snapshots
     storage.s3.host                                 leofs.licenser.net
storage.s3.secret_ke                                                ...
storage.s3.image_buc                                        fifo-images
```

### sniffle-admin config set `key` `value`
Sets a global config value please see the followign sections for valid settings.

### storage.general.backend
The backend used to store images, this can either be `internal` or `s3`. This needs to be set to `s3` to enable backups, `internal` only supports storing datasets / images.

### storage.s3.host
The host for the S3 this needs to be a domainname to propperly work, the domainname either needs to resolve or be in the `/etc/hsots`.

### storage.s3.port
The port for the S3's HTTPS interface, place note that HTTP is not supported.

### storage.s3.access_key
The access key for the S3 system.

### storage.s3.secret_key
The secret key for the S3 system.

### storage.s3.general_bucket
The bucket used for unknown data (currently unused).

### storage.s3.image_bucket
The bucket used for datasets/images.

### storage.s3.snapshot_bucket
The bucket used for backups, the bakups are stored in the format `<vm uuid>/<backup uuid>`.

## General tasks

### sniffle-admin hypervisors <subcommand>
* list - Lists all avaiable hypervisors
* delete `<uuid>` - removes a hypervisor

### sniffle-admin vms
* list - lists all VM's
* delete `<uuid>` - deletes a VM

### sniffle-admin packages
* list - lists all Packages
* delete `<uuid>` - deletes a Package

### sniffle-admin datasets
* list - lists all Datasets
* delete `<uuid>` - deletes a Dataset

