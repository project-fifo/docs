---
layout: default
title: snarl administration
---
The snarl admin command is `/opt/local/fifo-snarl/bin/snarl-admin` but many commands can also be accessed via `fifoadm` command. Please keep in mind that `fifoadm` is not designed as a every day command but only as a last fallback when commands are not available through the API.


## General managemnet
snarl uses the SMF to manage it's running state so it is restarted in the case of crashes and booted accordingly on system start. snarl can be enabled, disabled and restaerted via: `svcadm enable snarl`, `svcadm disable snarl` and `svcadm restart snarl`

### updating
snarl can be updated by three simple steps.

#### Installing the new package

```bash
pkgin -fy update
pkgin -fy install fifo-snarl
```

#### updating the config
After the newest package is installed the config file should be checked for changes and eddited if needed. The `.example` file will always contain the newest version of the config `diff` is a handy tool to see if some settings need to be added to the existing file.

```bash
diff /opt/loca/fifo-snarl/etc/snarl.conf /opt/loca/fifo-snarl/etc/snarl.conf.example
vi /opt/loca/fifo-snarl/etc/snarl.conf
```

#### restarting the service
After the config is updated the service needs to be restarted, snarl is running clustered and has more then `N` it is often possible to do a rolling update by restarting one by one.

## Cluster management

### snarl-admin join `<nodename>@<ip>`
Joins a snarl cluster, please note that all data on the joining (not the joined) node is deleted.

### snarl-admin leave
Cleanly removes a node from the ring. This is helpful when nodes get moved and the cluster downsized

### snarl-admin remove `<nodename>@<ip>`
Forcefully removes a node from the ring. This can be used after fatal node cluster.

### snarl-admin member-status
Lists the status of each node and the distribution of data over the ring nodes.

```
================================= Membership ==================================
Status     Ring    Pending    Node
-------------------------------------------------------------------------------
valid     100.0%      --      'snarl@172.16.0.254'
-------------------------------------------------------------------------------
Valid:1 / Leaving:0 / Exiting:0 / Joining:0 / Down:0
```

### snarl-admin ring-status
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

### snarl-admin status
A simple command that returns the overall cluster status, it returns a propper return code and is useful for scripted rolling updates.


### snarl-admin aae-status

Gives a detailed status on the AAE status of the system.

## Log Files
FiFo uses extensive logging to make debugging issue and understanding behaviour. The log files are located in `/var/log/snarl/`. There are multiple log files with various severities.


### debug.log
By default the debug log is disabled, it is very verbose and should not be enabled in production systems to enable it uncomment the followig line in the `snarl.conf`

```
log.debug.file = /var/log/snarl/debug.log
```

### console.log
This file contains logs of the level info and above, usually all interesting logs can be found here.

### error.log
This files contains errors, it usually should be mostly empty but please keep in mind that failing is not a uncommon practice to deal with unexpected behavuiour so sporadic entries might just be fine.



## Global configuration
In addition to the config files that apply on a per node level there are global configurations that can be changed from one system and are applied globally. Unless otherwise noted all those settings can be changed during runtime.

### snarl-admin config show
Shows a list of all settings in the global configuration.

### snarl-admin config set `key` `value`
Sets a global config value please see the followign sections for valid settings.

### defaults.users.inital_group
If this is set every user created gets placed into the group given. The value must be the UUID of the group.

### defaults.users.inital_org
If this is set every user created gets placed into the organisation given. The value must be the UUID of the organisation.

### yubico.api.client_id
When two factor authenticaiton is wanted this mist be set to the yubico API client ID.

### yubico.api.secret_key
When two factor authenticaiton is wanted this mist be set to the yubico API secret key.

## General tasks

### snarl-admin `users|group` add `<name>`
Adds a user or group, especially helpful when no users exist yet.

### snarl-admin `users|group` grant `<name>` `<permission>` `[<permission>]`
Grants a user or group permissions. A permission can have multiple epements so: `snarl-admin users grant user some special permissions` would grant `some->special->permissions` to `user`. `...` and `_` are special cases and not taken as strings but as the wildcards for the permission section where `_` means this level matches and `...` means evertying below this level matches.

### snarl-admin `users` passwd `<username>` `<password>`
Changes the password for a user.

### snarl-admin `users|groups|orgs|` list
Lists all users, groups or organisations.

### snarl-admin `users|groups|orgss|` delete `<name>`
Delets a given user, group or organisation.
