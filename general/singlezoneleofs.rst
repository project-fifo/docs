.. Project-FiFo documentation master file, created by
   Mark Slatem on Tue Nov 04 21:47:30 2014.


LeoFS Single Zone Install
#########################

LeoFS is a highly scalable, fault-tolerant object storage for the Web. LeoFS supports a huge amount of various kinds of unstructured data such as photo, movie, document, log data and so on.

In future versions of FiFo, LeoFS will be a mandatory requirement. Currently if you are using version 0.6.0 and are not using LeoFS, you are missing out on some incredible new features that 0.6.0 + LeoFS provides. Namely:

* The ability to backup and restore machines.
* The ability to archive machines.
* The ability to Freeze machines and re-deploy them to alternative nodes.
* the ability to connect to your S3 storage with S3 compatible clients.
  
The below guide is for people who want to run FiFo + LeoFS in a **single zone setup** e.g. Home, Lab or non-mission critical environment. This setup provides **NO** fault tolerance or high availability. For production use, the recommended setup is a distributed LeoFS cluster - deployed on 5 separate servers across 5 different zones.

DNS setup
=====================

Below is a sample of dns hostnames, that will need to resolve to the ip address of your FiFo Zone. The easiest way to accomplish this, is to add the DNS entries to a central DNS server that both the SmartOS Nodes as well as your FiFo zone uses for DNS resolution. 

.. code-block:: bash

  10.44.0.240 s3.onyxit.net
  10.44.0.240 fifo.s3.onyxit.net
  10.44.0.240 fifo-images.s3.onyxit.net
  10.44.0.240 fifo-snapshots.s3.onyxit.net

Global zone manual DNS configuration
------------------------------------

Please follow the normal **chunter** install process outlined in the official install guide. (`directions here <../chunter/installation.html>`_). Once chunter is running on your server, you will need to confirm that the GZ can resolve the DNS entries for your LeoFS storage.

If you dont have a local DNS server you should configure all your SmartOS Nodes to use the chunter provided SMF hosts service. This will ensure that the correct DNS entries are added to /etc/hosts file and will persist after server reboots. In the below step you will need to add your DNS entries to the ``/opt/chunter/etc/hosts`` file.
 

.. code-block:: bash
  
  cp /opt/chunter/share/hosts.xml /opt/custom/smf/
  vi /opt/chunter/etc/hosts
  svccfg validate /opt/custom/smf/hosts.xml
  svccfg import /opt/custom/smf/hosts.xml

LeoFS dataset install
============================

Fortunately there is a LeoFS dataset available, that has specially been created for running FiFo in a **single zone** non clustered environment. This dataset will by default have all the LeoFS packages installed and running for us. 

Note: **Do NOT** use this dataset for a clustered FiFo installation.

First we need to change our ``imgadm`` repository and then import the LeoFS dataset we will use for the FiFo zone.

.. code-block:: bash

   imgadm sources -a http://datasets.at/
   imgadm sources -d https://images.joyent.com
   imgadm update
   imgadm import ccd69a86-ae83-11e4-813b-371265fbe00e

Next we create a JSON payload file to create our zone.

.. code-block:: bash

   cd /opt
   vi /opt/newfifo.json

Here is the JSON setting you will need for the ``newfifo.json`` file.

.. code-block:: json

   {
    "autoboot": true,
    "brand": "joyent",
    "image_uuid": "ccd69a86-ae83-11e4-813b-371265fbe00e",
    "max_physical_memory": 4096,
    "cpu_cap": 100,
    "alias": "Project-FiFo",
    "hostname": "fifo.onyxit.net",
    "quota": "200",
    "resolvers": [
    "8.8.8.8",
    "8.8.4.4"
    ],
    "nics": [
    {
    "interface": "net0",
    "nic_tag": "admin",
    "gateway": "10.44.0.1",
    "ip": "10.44.0.240",
    "netmask": "255.255.255.0"
    }
    ]
   }

Last step here is to create the zone. The rest of the setup will be done within our newly created fifo zone.

::
   
   [root@00-25-90-71-af-64 /opt]# vmadm create -f newfifo.json
   Successfully created VM df43608e-0aca-4bfe-a31a-85024d6b5a36

Zone health check
==================

Login to your new zone and lets continue with the rest of the setup.

:: 

   [root@00-25-90-71-af-64 /opt]# zlogin df43608e-0aca-4bfe-a31a-85024d6b5a36
   [Connected to zone 'df43608e-0aca-4bfe-a31a-85024d6b5a36' pts/8]
           _ __
        __( =  =- _           ,__.   ,__.
       (-       -  )__- -_    |__. . |__. .--.
      (  -=  - )   -     _)   |    | |    |  |
     (_-= _(    =-    _=-     '    ' '    `--'
      -(     -    -  _)   Instance (leofs 1.1.5)
        -=__(__  _-)-     http://loefs.org
              -=-
   [root@fifo ~]# 


First lets make sure and confirm that all the LeoFS services are up and running and working correctly.

::

   [root@fifo ~]# svcs | grep leofs
   online          5:46:32 svc:/leofs/storage:default
   online          5:46:32 svc:/leofs/manager1:default
   online          5:46:32 svc:/leofs/manager0:default
   online          5:46:35 svc:/leofs/gateway:default
   [root@fifo ~]# leofs-adm status
   [System config]
                   System version : 1.1.5
                       Cluster Id : leofs_1
                            DC Id : dc_1
                   Total replicas : 1
              # of successes of R : 1
              # of successes of W : 1
              # of successes of D : 1
    # of DC-awareness replicas    : 0
                        ring size : 2^128
                Current ring hash : 8a86be90
                   Prev ring hash : 8a86be90
   [Multi DC replication settings]
            max # of joinable DCs : 2
               # of replicas a DC : 1
   
   [Node(s) state]
   -------+--------------------------+--------------+----------------+----------------+   ----------------------------
    type  |           node           |    state     |  current ring  |   prev ring     |          updated at
   -------+--------------------------+--------------+----------------+----------------+   ----------------------------
     S    | storage_0@127.0.0.1      | running      | 8a86be90       | 8a86be90        | 2014-11-04 05:45:59 +0000
     G    | gateway_0@127.0.0.1      | running      | 8a86be90       | 8a86be90        | 2014-11-04 05:46:37 +0000
   
Confirm you can ping all your hostnames and that they correctly resolve to your FiFo zone ip address. If not add the dns entries manually to the fifo zone's ``/etc/hosts`` file.

::

  10.44.0.240 s3.onyxit.net
  10.44.0.240 fifo.s3.onyxit.net
  10.44.0.240 fifo-images.s3.onyxit.net
  10.44.0.240 fifo-snapshots.s3.onyxit.net   

S3 bucket & key creation
========================

Next we add our endpoint and create the S3 compatible storage buckets. This is what FiFo will use for its object storage. We will generate a random password with ``ssl`` which we will use to seed the key creation. 

.. note::  Do **NOT** lose your **access-key-id** and **secret-access-key** as you will need them later to complete your setup.

::

   leofs-adm add-endpoint s3.onyxit.net
   openssl rand -base64 32 | fold -w16 | head -n1
   leofs-adm create-user fifo qypdpQ47e/E4oKH3

   access-key-id: ed4528b19bc043770c12
   secret-access-key: 18b35c2dc5b0819e31d7c2fece24add0ef9ec221 

Next we create our 3 buckets using our ``access-key-id`` Then we install all our FiFo services.

:: 

   leofs-adm add-bucket fifo ed4528b19bc043770c12
   leofs-adm add-bucket fifo-images ed4528b19bc043770c12
   leofs-adm add-bucket fifo-snapshots ed4528b19bc043770c12

   echo "http://release.project-fifo.net/pkg/rel/" >>/opt/local/etc/pkgin/repositories.conf
   pkgin -fy up
   pkgin install nginx fifo-snarl fifo-sniffle fifo-howl fifo-wiggle fifo-jingles
   cp /opt/local/fifo-jingles/config/nginx.conf /opt/local/etc/nginx/nginx.conf


Verify ports & install packages
===============================

The most important part of this step is to make sure you change the ports to something that does not conflict with the default ports used by the other FiFo services. I always tend to use ``8184`` for http and ``7443`` for https as I know these work and do not conflict.

Change the following in ``/opt/local/leofs/leo_gateway/etc/leo_gateway.conf``

::

   http.port = 8184
   http.ssl_port = 7443

Then change the following in ``/opt/local/fifo-sniffle/etc/sniffle.conf``

::

   s3.host = 10.44.0.240:7443

Now proceed to restart the Leofs gateway and startup the FiFo services and confirm they are all running correctly.

::

   svcadm restart leofs/gateway

   svcadm enable epmd
   svcadm enable snarl
   svcadm enable sniffle
   svcadm enable howl
   svcadm enable wiggle
   svcadm enable nginx
   svcs epmd snarl sniffle howl wiggle nginx

Sniffle-admin config
===========================

We are almost done. All that is left, is to tell sniffle to use our S3 LeoFS storage buckets and to grant access by specifying which ``access_key`` and ``secret_key`` to connect with.

::

   sniffle-admin config set storage.s3.port 7443
   sniffle-admin config set storage.general.backend s3
   sniffle-admin config set storage.s3.host s3.onyxit.net
   sniffle-admin config set storage.s3.access_key ed4528b19bc043770c12
   sniffle-admin config set storage.s3.secret_key 18b35c2dc5b0819e31d7c2fece24add0ef9ec221
   sniffle-admin config set storage.s3.image_bucket fifo-images
   sniffle-admin config set storage.s3.general_bucket fifo
   sniffle-admin config set storage.s3.snapshot_bucket fifo-snapshots
   
   sniffle-admin config show

Last step is to add a FiFo user and password, then you are done.

::

   fifoadm users add default admin
   fifoadm users grant default admin ...
   fifoadm users passwd default admin admin

Login to the web interface ``http://ip_or_hostname`` and enjoy your new cutting edge FiFo setup!

.. toctree::
   :glob:
