.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

Installation
############

.. attention::

   Please be aware that this guide covers the installation of a **single** *FiFo Zone*. While this is supported and might be acceptable for a private server a production environment should consist of **at least 5 physically separated** zones!


.. warning::

   FiFo does not run well inside virtualized environments such as VMWare or virtualbox, their virtualized networking can interfeer with FiFo's auto discovery.


.. seealso::

  Details on how to set up clustering can be found in the `Clustering <clustering.html>`_ section.

____


Creating the Zone
-----------------


From the *GZ (Global Zone)* we install the base dataset which we will use for our *FiFo Zone*. Than we have to confirm it is installed:


.. code-block:: bash

   imgadm update
   imgadm import d34c301e-10c3-11e4-9b79-5f67ca448df0


If installed successfully you should see:

.. code-block:: bash

   d34c301e-10c3-11e4-9b79-5f67ca448df0  base64        14.2.0    smartos  2014-07-21T10:43:17Z

Sample contents of ``setupfifo.json``


.. code-block:: json

   {
    "autoboot": true,
    "brand": "joyent",
    "image_uuid": "d34c301e-10c3-11e4-9b79-5f67ca448df0",
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

We now *zlogin* to our newly created *FiFo Zone* and proceed with adding the *FiFo* package repository and then installing the *FiFo* packages

.. code-block:: bash

   zlogin <fifo-vm-uuid>
   VERSION=rel
   echo "http://release.project-fifo.net/pkg/${VERSION}/" >>/opt/local/etc/pkgin/repositories.conf
   pkgin -fy up
   pkgin install nginx fifo-snarl fifo-sniffle fifo-howl fifo-wiggle fifo-jingles
   cp /opt/local/fifo-jingles/config/nginx.conf /opt/local/etc/nginx/nginx.conf

.. note::

  - To install the release version use `VERSION=rel`
  - To install the current development version use `VERSION=dev`

____


Configuration
-------------

If this is a fresh installation the installer will create default configuration files for each service. When updating the configuration files do not get overwritten but new ``*.conf.example`` files will be added. 
The generated files contain some defaults. However is it advised to take some time to configure `Wiggle <../wiggle/configuration.html>`_, `Sniffle <../sniffle/configuration.html>`_, `Snarl <../snarl/configuration.html>`_ and `Howl <../howl/configuration.html>`_.

____


Startup
-------

.. code-block:: bash

   svcadm enable epmd
   svcadm enable snarl
   svcadm enable sniffle
   svcadm enable howl
   svcadm enable wiggle
   svcadm enable nginx
   svcs epmd snarl sniffle howl wiggle nginx

____


Initial administrative tasks
----------------------------

The last step is to create an *admin user* granted unrestricted permissions so we can login. It is important to ensure that a permission called ``...`` is added, which assigns "ALL usage rights" to your admin user.

.. code-block:: bash

   fifoadm users add default admin
   fifoadm users grant default admin ...
   fifoadm users passwd default admin admin


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


That's it. You can now log out of your *FiFo Zone* and back into the *Global Zone* and continue with installing the *Chunter* service (`directions here <../chunter/installation.html>`_).
