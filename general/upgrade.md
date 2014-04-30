---
layout: default
category: general
section: general/upgrade
title: Project-FiFo Upgrade Guide
---

# General instructions

The General instructions cover steps that will initiate the update, please be aware that depending on your update path additional steps might be required. The version specific section covers the steps required to go from one version to another. While you're on release following these on a version update is enough. Things are a bit more complicated when living on the bleeding edge (aka dev) is a bit more complicated, in general you should check here on every update since required steps for a version upgrade will slowly grow during the development process.


## Zone
To upgrade the components in a zone it's nessessart to install the new packages and restart the services, in addition to this sometimes additional steps are needed depended on the update.

```bash
pkgin -fy up
pkgin install fifo-snarl fifo-sniffle fifo-howl fifo-wiggle fifo-jingles
svcadm restart sniffle
svcadm restart snarl
svcadm restart howl
svcadm restart wiggle
```

## Hypervisors

There are two ways to update the server on the hypervisor one is to connect to the hypervisor and run:

```bash
/opt/chunter/bin/update
```

It's always good to confirm chunter is running:

```bash
svcs chunter
STATE          STIME    FMRI
online         21:16:28 svc:/network/chunter:default
```

An alternative is to use fifoadm to update a hypervisor by running:

```bash
fifoadm hypervisors update
```

this will trigger all hypervisors to update.


## 0.4.4<a id="0.4.4"></a>

With 0.4.4 there is a considerable update of the database this means additional steps need to be taken. Once all serivces have been updated the following commands need to be run:

<p class="bs-callout bs-callout-danger">
It is critical that <b>ALL</b> services are running and connected during this update otherwise dataloss can occour!
</p>

```
/opt/loca/fifo-sniffle/bin/sniffle-admin db update
/opt/loca/fifo-snarl/bin/snarl-admin db update
```

## 0.4.3<a id="0.4.3"></a>

This version introduces a new system for config files. The aim is to make fifo more ops friendly by providing more human readable configuration with documentation.

<p class="bs-callout bs-callout-danger">
The old files will conflict with the existing ones so it is important to transfair the changes form the old files and adjust them accordingly in the new files then <b>delete</b> the old files.
</p>

* **Chunter**

    The old files are `/opt/chunter/etc/sys.config` and `/opt/chunter/etc/app.config` which are replaced by `/opt/chunter/etc/chunter.conf`

* **Sniffle**

    The old files are `/opt/local/fifo-sniffle/etc/sys.config` and `/opt/local/fifo-sniffle/etc/app.config` which are replaced by `/opt/local/fifo-sniffle/etc/sniffle.conf`

* **Snarl**

    The old files are `/opt/local/fifo-snarl/etc/sys.config` and `/opt/local/fifo-snarl/etc/app.config` which are replaced by `/opt/local/fifo-snarl/etc/snarl.conf`

* **Howl**

    The old files are `/opt/local/fifo-howl/etc/sys.config` and `/opt/local/fifo-howl/etc/app.config` which are replaced by `/opt/local/fifo-howl/etc/howl.conf`

* **Wiggle**

    The old files are `/opt/local/fifo-wiggle/etc/sys.config` and `/opt/local/fifo-wiggle/etc/app.config` which are replaced by `/opt/local/fifo-wiggle/etc/wiggle.conf`


* **Jingles**

    The location of the jingles has changed that means the nginx config has to be changed or the new templated to be used, details can be found in the message printed during installation.
