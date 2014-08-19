.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**********
Upgradeing
**********

General instructions
####################

The General instructions cover steps that will initiate the update, please be aware that depending on your update path additional steps might be required. The version specific section covers the steps required to go from one version to another. While you're on release following these on a version update is enough. Things are a bit more complicated when living on the bleeding edge (aka dev) is a bit more complicated, in general you should check here on every update since required steps for a version upgrade will slowly grow during the development process.

.. warning::

   It is highly advised to make a snapshot of the FiFo zone before updating, while the procedure should be safe it is always better to stay on the side of caution.



Zone
----

To upgrade the components in a zone it's necessary to install the new packages and restart the services, in addition to this sometimes additional steps are needed depended on the update.

.. code-block:: bash

   pkgin -fy up
   pkgin install fifo-snarl fifo-sniffle fifo-howl fifo-wiggle fifo-jingles
   svcadm restart sniffle
   svcadm restart snarl
   svcadm restart howl
   svcadm restart wiggle


Hypervisors
-----------

There are two ways to update the server on the hypervisor one is to connect to the hypervisor and run:

.. code-block:: bash

   /opt/chunter/bin/update

It's always good to confirm chunter is running:

.. code-block:: bash

   svcs chunter
   STATE          STIME    FMRI
   online         21:16:28 svc:/network/chunter:default

An alternative is to use fifoadm to update a hypervisor by running:

.. code-block:: bash

   fifoadm hypervisors update

this will trigger all hypervisors to update.

0.6.0
-----

Version 0.6.0 of fifo introduces a version of Snarl that allows to have multiple parallel authentication realms inside of Snarl. To support this the former global information into a realm this can be archived by running the DB update command:

.. warning::

   It is critical that **ALL** services are running and connected during this update otherwise data loss can occur!

.. code-block:: bash

   snarl-admin db update default


This will place all users, roles and organizations into the ``default`` realm. Another realm can be chosen but it will require configuring the remaining fifo services to use this.

0.4.4
-----

With 0.4.4 there is a considerable update of the database this means additional steps need to be taken. Once all services have been updated the following commands need to be run:

.. warning::

   It is critical that **ALL** services are running and connected during this update otherwise data loss can occur!

.. code-block:: bash

   sniffle-admin db update
   snarl-admin db update

This changes also affect the AAE code, this means when AAE is enabled the old AAE data needs to be deleted, this has no impact on the system itself. The services should be disabled when the AAE data is deleted:

.. code-block:: bash

   rm -r /var/db/sniffle/anti_entropy
   rm -r /var/db/snarl/anti_entropy

0.4.3
-----

This version introduces a new system for config files. The aim is to make fifo more ops friendly by providing more human readable configuration with documentation.

.. warning::

   The old files will conflict with the existing ones so it is important to transfer the changes form the old files and adjust them accordingly in the new files then **delete** the old files.

* **Chunter**

    The old files are ``/opt/chunter/etc/sys.config`` and ``/opt/chunter/etc/app.config`` which are replaced by ``/opt/chunter/etc/chunter.conf``

* **Sniffle**

    The old files are ``/opt/local/fifo-sniffle/etc/sys.config`` and ``/opt/local/fifo-sniffle/etc/app.config`` which are replaced by ``/opt/local/fifo-sniffle/etc/sniffle.conf``

* **Snarl**

    The old files are ``/opt/local/fifo-snarl/etc/sys.config`` and ``/opt/local/fifo-snarl/etc/app.config`` which are replaced by ``/opt/local/fifo-snarl/etc/snarl.conf``

* **Howl**

    The old files are ``/opt/local/fifo-howl/etc/sys.config`` and ``/opt/local/fifo-howl/etc/app.config`` which are replaced by ``/opt/local/fifo-howl/etc/howl.conf``

* **Wiggle**

    The old files are ``/opt/local/fifo-wiggle/etc/sys.config`` and ``/opt/local/fifo-wiggle/etc/app.config`` which are replaced by ``/opt/local/fifo-wiggle/etc/wiggle.conf``


* **Jingles**

    The location of the jingles has changed that means the nginx config has to be changed or the new templated to be used, details can be found in the message printed during installation.
