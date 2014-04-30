---
layout: default
category: general
section: general/installation
title: Project-FiFo configuration
---

<p class="bs-callout bs-callout-danger">
Please be aware that this guide covers the installation of a <b>single</b> FiFo zone, while this is suppored and might be acceptable for a prive 'for fun' server a production environment should consist of <b>at least 5 physically seperated</b> zone! Details on how to set up clustering can be found in the <a href="/general/clustering.html">Clustering</a> section.
</p>

## Creating the Zone
From the GZ (Global Zone) we install the base dataset we are going to use for our FiFo Zone and confirm its is installed:

```bash
imgadm update
imgadm import 17c98640-1fdb-11e3-bf51-3708ce78e75a
imgadm list | grep 17c98640
```

If installed successfully you should see:

```
17c98640-1fdb-11e3-bf51-3708ce78e75a  base64  13.2.1   smartos  2013-09-20T15:02:46Z
```

Sample contents of setupfifo.json

```json
{
 "autoboot": true,
 "brand": "joyent",
 "image_uuid": "17c98640-1fdb-11e3-bf51-3708ce78e75a",
 "max_physical_memory": 3072,
 "cpu_cap": 100,
 "alias": "fifo",
 "quota": "40",
 "resolvers": [
 "8.8.8.8",
 "8.8.4.4"
 ],
 "nics": [
 {
 "interface": "net0",
 "nic_tag": "admin",
 "ip": "10.1.1.240",
 "gateway": "10.1.1.1",
 "netmask": "255.255.255.0"
 }
 ]
}
```

Next we create our FiFo JSON payload file and save it incase we need to reinstall again at a later stage.

```bash
cd /opt
vi setupfifo.json
vmadm create -f setupfifo.json
```


## Installing the services

We now zlogin to our newly created FiFo zone and proceed with adding the FiFo package repository and then installing the FiFo packages

<p class="bs-callout bs-callout-info">
Please note that this is for the reelease version of FiFo to installed the current development version use `VERSION=dev` instead of `VERSION=rel`.
</p>


```bash
zlogin <fifo-vm-uuid>
VERSION=rel
echo "http://release.project-fifo.net/pkg/${VERSION}/" >>/opt/local/etc/pkgin/repositories.conf
pkgin -fy up
pkgin install nginx fifo-snarl fifo-sniffle fifo-howl fifo-wiggle fifo-jingles
cp /opt/local/fifo-jingles/config/nginx.conf /opt/local/etc/nginx/nginx.conf
```

## Configuration
If this is a fresh installation the installer will create default configuration files for each service. When updating the configuration files do not get overwritten but new `*.conf.example` files are added. The generated files contain some defaults it non the less adviced to take some time to configure [wiggle](/wiggle/configuration.html), [sniffle](/sniffle/configuration.html), [snarl](/snarl/configuration.html) and [howl](/howl/configuration.html).


## Startup
```bash
svcadm enable epmd
svcadm enable snarl
svcadm enable sniffle
svcadm enable howl
svcadm enable wiggle
svcadm enable nginx
svcs epmd snarl sniffle howl wiggle nginx
```

## Initial administrative tasks
The last step is to create an admin user with full permissions so we can login. The important part is to ensure that a permission called `...` is added, which assigns "ALL usage rights" to your admin user.

```
fifoadm users add admin
fifoadm users grant admin ...
fifoadm users passwd admin admin
```

If you want to add a default user group execute the following commands to assign basic permissions to the group so that users belonging to this group can create and manage their own vm's.

<p class="bs-callout bs-callout-info">
In 0.4.4 groups got renamed to roles if you are on dev this commands need to be changed accordingly.
</p>

```
fifoadm groups add Users
fifoadm groups grant Users cloud cloud status
fifoadm groups grant Users cloud datasets list
fifoadm groups grant Users cloud networks list
fifoadm groups grant Users cloud ipranges list
fifoadm groups grant Users cloud packages list
fifoadm groups grant Users cloud vms list
fifoadm groups grant Users cloud vms create
fifoadm groups grant Users hypervisors _ create
```

That's it. You can now log out of your "fifo" zone and back into the global zone and continue with installing the "chunter" service.
