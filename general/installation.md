---
layout: default
category: general
section: general/installation
title: Project-FiFo configuration
---

<p class="bs-callout bs-callout-danger">
Please be aware that this guide covers the installation of a <b>single</b> FiFo zone, while this is supported and might be acceptable for a private 'for fun' server a production environment should consist of <b>at least 5 physically separated</b> zone! Details on how to set up clustering can be found in the <a href="/general/clustering.html">Clustering</a> section.
</p>

## Creating the Zone
From the GZ (Global Zone) we install the base dataset we are going to use for our FiFo Zone and confirm its is installed:

```bash
imgadm update
imgadm import dc0688b2-c677-11e3-90ac-13373101c543
imgadm list | grep dc0688b2
```

If installed successfully you should see:

```
dc0688b2-c677-11e3-90ac-13373101c543  base64  13.4.2   smartos  2013-09-20T15:02:46Z
```

Sample contents of setupfifo.json

```json
{
 "autoboot": true,
 "brand": "joyent",
 "image_uuid": "dc0688b2-c677-11e3-90ac-13373101c543",
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

Next we create our FiFo JSON payload file and save it in case we need to reinstall again at a later stage.

```bash
cd /opt
vi setupfifo.json
vmadm create -f setupfifo.json
```


## Installing the services

We now zlogin to our newly created FiFo zone and proceed with adding the FiFo package repository and then installing the FiFo packages

<p class="bs-callout bs-callout-info">
Please note that this is for the release version of FiFo to installed the current development version use `VERSION=dev` instead of `VERSION=rel`.
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
If this is a fresh installation the installer will create default configuration files for each service. When updating the configuration files do not get overwritten but new `*.conf.example` files are added. The generated files contain some defaults it non the less advised to take some time to configure [wiggle](/wiggle/configuration.html), [sniffle](/sniffle/configuration.html), [snarl](/snarl/configuration.html) and [howl](/howl/configuration.html).


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

<p class="bs-callout bs-callout-info">
Starting with 0.6.0 (current dev) Snarl supports multiple realms, unless otherwise configured fifo will use the 'default' realm.</br>
In consequence this means all user and roll commands need an additional argument, the realm is specified as first argument behind the command so it changes as follows:</br>
'fifoadm users add admin' becomes 'fifoadm users add default admin'.
</p>


The last step is to create an admin user with full permissions so we can login. The important part is to ensure that a permission called `...` is added, which assigns "ALL usage rights" to your admin user.

```
fifoadm users add admin
fifoadm users grant admin ...
fifoadm users passwd admin admin
```

If you want to add a default user role execute the following commands to assign basic permissions to the role so that users belonging to this role can create and manage their own vm's.


```
fifoadm roles add Users
fifoadm roles grant Users cloud cloud status
fifoadm roles grant Users cloud datasets list
fifoadm roles grant Users cloud networks list
fifoadm roles grant Users cloud ipranges list
fifoadm roles grant Users cloud packages list
fifoadm roles grant Users cloud vms list
fifoadm roles grant Users cloud vms create
fifoadm roles grant Users hypervisors _ create
fifoadm roles grant Users datasets _ create
fifoadm roles grant Users roles <uuid of Users role> get
```

That's it. You can now log out of your "fifo" zone and back into the global zone and continue with installing the "chunter" service.
