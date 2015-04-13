.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**********
Upgrading
**********

General instructions
####################

These instructions cover steps that will initiate the update. Please be aware that depending on your update path additional steps might be required. The version specific section covers the steps required to go from one version to another. While you're on release following these steps on a version update is enough. Things are a bit more complicated when living on the bleeding edge (aka dev). In general you should check here on every update since required steps for a version upgrade will slowly grow during the development process.

.. warning::

   It is highly advised to make a snapshot of the *FiFo Zone* before updating. While the procedure should be safe it is always better to stay on the side of caution.

Zone
****

To upgrade the components in a zone it is necessary to install the new packages and restart the services. In addition to this there are sometimes additional steps needed depending on the update.

.. code-block:: bash

   pkgin -fy up
   pkgin install fifo-snarl fifo-sniffle fifo-howl fifo-jingles
   svcadm restart sniffle
   svcadm restart snarl
   svcadm restart howl


Hypervisors
***********

.. note::
 
 There are two ways to update the server on the hypervisor.

1. Connect to the hypervisor and run:

 .. code-block:: bash

   /opt/chunter/bin/update

 It's always good to confirm `Chunter <../chunter.html>`_ is running:

 .. code-block:: bash

   svcs chunter
   STATE          STIME    FMRI
   online         21:16:28 svc:/network/chunter:default

2. Use ``fifoadm`` to update a hypervisor by running:

 .. code-block:: bash

   fifoadm hypervisors update

 This will trigger all hypervisors to update.

____

0.6.2
*****

FiFo 0.6.2 combines the functionality that was formaly spread out between wiggle howl and nginx into howl, this means neither nginx nor wiggle are any longer needed. Before updating stop and remove those services.

 .. code-block:: bash

   svcadm disable wiggle
   svcadm disable nginx
   pkgin remove fifo-wiggle nginx

If you already have howl isntalled you will need to grant the user additional permissions to be able to open reserved ports (http: 80, and https: 443) this can be done with the following command. New installations handle this grant as part of the installation routime.

.. code-block:: bash

   /usr/sbin/usermod -K defaultpriv=basic,net_privaddr howl

____

0.6.1
*****

When upgrading, the Snarl config file (``/opt/local/fifo-snarl/etc/snarl.conf``) will always includes the line ``folsom_ddb.ip = ...``. If your DDB has not been previously configured to collect FiFo metrics, then this line needs to be commented out or else the "snarl" service will go into maintenance. This will be the case for the majority of users and does not affect any other FiFo services.

____

0.6.0
*****

Version 0.6.0 of FiFo introduces a feature that allows for multiple parallel authentication realms inside of `Snarl <../snarl.html>`_. To support this the former global information into a realm can be archived by running the DB update command:

.. warning::

 It is critical that **ALL** services are running and connected during this update otherwise data loss can occur!

.. code-block:: bash

 snarl-admin db update default


This will place all users, roles and organizations into the ``default`` realm. Another realm can be chosen but it will require configuration of the remaining *FiFo* services.

____

0.4.4
*****

With 0.4.4 there is a considerable update to the database. Therefore additional steps need to be taken. Once all services have been updated the following commands need to be run:

.. warning::

 It is critical that **ALL** services are running and connected during this update otherwise data loss can occur!

.. code-block:: bash

 sniffle-admin db update
 snarl-admin db update

These changes also affect the *AAE code*. Therefore when AAE is enabled the old AAE data needs to be deleted. This has no impact on the system itself. 

.. attention::

  The services should be disabled when the AAE data is deleted!

.. code-block:: bash
 
 rm -r /var/db/sniffle/anti_entropy
 rm -r /var/db/snarl/anti_entropy

____

0.4.3
*****

This version introduces a new system for config files. The aim is to make *FiFo* more ops friendly by providing more human readable configuration with documentation.

.. attention::

 Old files will conflict with the existing ones so **it is important to transfer the changes form the old files, adjust them accordingly in the new files and then delete the old files**.

`Chunter <../chunter.html>`_
++++++++++++++++++++++++++++

The old files are ``/opt/chunter/etc/sys.config`` and ``/opt/chunter/etc/app.config`` which are replaced by ``/opt/chunter/etc/chunter.conf``

`Sniffle <../sniffle.html>`_
++++++++++++++++++++++++++++

The old files are ``/opt/local/fifo-sniffle/etc/sys.config`` and ``/opt/local/fifo-sniffle/etc/app.config`` which are replaced by ``/opt/local/fifo-sniffle/etc/sniffle.conf``

`Snarl <../snarl.html>`_
++++++++++++++++++++++++

The old files are ``/opt/local/fifo-snarl/etc/sys.config`` and ``/opt/local/fifo-snarl/etc/app.config`` which are replaced by ``/opt/local/fifo-snarl/etc/snarl.conf``

`Howl <../howl.html>`_
++++++++++++++++++++++

The old files are ``/opt/local/fifo-howl/etc/sys.config`` and ``/opt/local/fifo-howl/etc/app.config`` which are replaced by ``/opt/local/fifo-howl/etc/howl.conf``

`Wiggle <../wiggle.html>`_
++++++++++++++++++++++++++
 
The old files are ``/opt/local/fifo-wiggle/etc/sys.config`` and ``/opt/local/fifo-wiggle/etc/app.config`` which are replaced by ``/opt/local/fifo-wiggle/etc/wiggle.conf``


`Jingles <../jingles.html>`_
++++++++++++++++++++++++++++

**The location of the `Jingles <../jingles.html>`_ has changed.** Therefore the nginx config has to be changed or the new templated has to be used. *Details can be found in the message printed during installation*.
