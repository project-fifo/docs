.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

Installation
############

.. attention::

   Please be aware that this guide covers the installation of a **single** *FiFo Zone*. While this is supported and might be acceptable for a private server, a production environment should consist of **at least 5 physically separated** zones!


.. warning::

   FiFo does not run well inside virtualized environments such as VMWare or virtualbox, their virtualized networking can interfere with FiFo's auto discovery.


.. seealso::

  Details on how to set up clustering can be found in the `Clustering <clustering.html>`_ section.

The latest SmartOS version that is tested with FiFo is **20150108T111855Z** (`USB <https://us-east.manta.joyent.com/Joyent_Dev/public/SmartOS/20150108T111855Z/smartos-20150108T111855Z-USB.img.bz2>`_, `ISO <https://us-east.manta.joyent.com/Joyent_Dev/public/SmartOS/20150108T111855Z/smartos-20150108T111855Z-USB.iso>`_, `VMWare <https://us-east.manta.joyent.com/Joyent_Dev/public/SmartOS/20150108T111855Z/smartos-20150108T111855Z-USB.vmwarevm.tar.bz2>`_, `TGZ <https://us-east.manta.joyent.com/Joyent_Dev/public/SmartOS/20150108T111855Z/smartos-20150108T111855Z-USB.tgz>`_) a full list is available `here <https://github.com/project-fifo/chunter/blob/dev/rel/pkg/install.sh#L5>`_.

____


Creating the Zone
-----------------


From the *GZ (Global Zone)* we install the base dataset which we will use for our *FiFo Zone*. Than we have to confirm it is installed:


.. code-block:: bash

   imgadm update
   imgadm import 5a4ba06a-c1bb-11e4-af0b-4be0ce4ce04c


If installed successfully you should see:

.. code-block:: bash

   5a4ba06a-c1bb-11e4-af0b-4be0ce4ce04c  minimal-64-lts  14.4.0     smartos  2015-03-03T15:38:34Z

Sample contents of ``setupfifo.json``


.. code-block:: json

   {
    "autoboot": true,
    "brand": "joyent",
    "image_uuid": "5a4ba06a-c1bb-11e4-af0b-4be0ce4ce04c",
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


Next we create our *FiFo JSON* payload file and save it in case we need to reinstall at a later stage.

.. code-block:: bash

   cd /opt
   vi setupfifo.json
   vmadm create -f setupfifo.json

____



Installing the services
-----------------------

We now *zlogin* to our newly created *FiFo Zone* and proceed with adding the *FiFo* package repository and then installing the *FiFo* packages.

.. code-block:: bash

   zlogin <fifo-vm-uuid>


With the 14.4.0 Joyent introduced signed packages. With version 0.6.2 FiFo has also started signing it's packages, to properly install FiFo packages it is however required to install the `FiFo public key <https://project-fifo.net/fifo.gpg>`_ this can be done by the following commands. The key id is ``BB975564`` and the fingerprint is ``CE62 C662 67D5 9129 B291  62A0 ADDF 278A BB97 5564``

.. code-block:: bash

   curl -O https://project-fifo.net/fifo.gpg
   gpg --primary-keyring /opt/local/etc/gnupg/pkgsrc.gpg --import < fifo.gpg
   gpg --keyring /opt/local/etc/gnupg/pkgsrc.gpg --fingerprint

Now we can install the packages

.. code-block:: bash

   VERSION= # one of 'rel' or 'dev'
   echo "http://release.project-fifo.net/pkg/${VERSION}/" >>/opt/local/etc/pkgin/repositories.conf
   pkgin -fy up
   pkgin install fifo-snarl fifo-sniffle fifo-howl fifo-jingles fifo-watchdog

.. note::

  - To install the release version use `VERSION=rel`
  - To install the current development version use `VERSION=dev`

____


Configuration
-------------

If this is a fresh installation the installer will create default configuration files for each service. When updating the configuration files do not get overwritten but new ``*.conf.example`` files will be added.
The generated files contain some defaults. However is it advised to take some time to configure `Sniffle <../sniffle/configuration.html>`_, `Snarl <../snarl/configuration.html>`_ and `Howl <../howl/configuration.html>`_.

Ple
____


Startup
-------

.. code-block:: bash

   svcadm enable epmd
   svcadm enable snarl
   svcadm enable sniffle
   svcadm enable howl
   svcadm enable watchdog
   svcs epmd snarl sniffle howl

____


Initial administrative tasks
----------------------------

The last step is to create an *admin user* and organisation, this can be done with one simple command:

.. code-block:: bash

   # snarl-admin init <realm> <org> <user> <pass>
   snarl-admin init default MyOrg admin admin



If you want to add a default user role execute the following commands to assign basic permissions to the role so that users belonging to this role can create and manage their own vm's.

.. code-block:: bash

   fifoadm roles add default Users
   fifoadm roles grant default Users cloud cloud status
   fifoadm roles grant default Users cloud datasets list
   fifoadm roles grant default Users cloud networks list
   fifoadm roles grant default Users cloud ipranges list
   fifoadm roles grant default Users cloud packages list
   fifoadm roles grant default Users cloud vms list
   fifoadm roles grant default Users cloud vms create
   fifoadm roles grant default Users hypervisors _ create
   fifoadm roles grant default Users datasets _ create
   fifoadm roles grant default Users roles <uuid of Users role> get


LeoFS
-----

.. warning::

   S3 does require a host name or FQDN to work, ip addresses are not working to access the store. Both a DNS server and entries in ``/etc/hosts`` work.

`XIP.io <http://xip.io>`_ is a good alternative for test systems, it resolves hostnames in the form of: ``*.<io>.xip.io`` to ``<ip>`` so if the LeoIP is ``10.0.0.100`` using ``10.0.0.100.xip.io`` as a hostname for the storage server will work..

ProjectFiFo provides packages for LeoFS in it's repository, ``leo_manager``, ``leo_storage`` and ``leo_gateway``. Here the ``leo_manager`` package is used for both the **master** and **slave** manager!

For details on how to set up LeoFS see the official `LeoFS manual <http://leo-project.net/leofs/docs/configuration/configuration.html>`_

Once LeoFS is configured the ``init-leofs`` command can be used from ``sniffle-admin`` to set up the required, users, buckets and endpoints.

.. code-block:: bash

   sniffle-admin init-leofs leo.fifo.net

That's it. You can now log out of your *FiFo Zone* and back into the *Global Zone* and continue with installing the *Chunter* service (`directions here <../chunter/installation.html>`_).

