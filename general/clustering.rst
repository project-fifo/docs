.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

Clustering
##########

Application Types
*****************

*FiFo* is built to maintain the highest possible uptime: this means all components fall into one of two categories:

Stateless
*********

Maintaining availability in stateless applications is pretty simple, it's 'the cloud way': you can fire up multiple instances and since they don't need to share state a failing instance can simply be replaced by a new one or discarded alltogether.

The following components are stateless:

- `Wiggle <../wiggle.html>`_
- `Jingles <../jingles.html>`_

To maintain availability you will have to spawn at least two of each and put them behind a load balancer. In the current version this task has to be performed by the user.

Cluster/Distribution
********************

Some components can't be stateless since they need to maintain some kind of data. To achieve a high level of availability these components run in a clustered/distributed mode. *FiFo* uses *riak core* as a distribution layer. This means that it follows an eventual consistent model and utilises all nodes that are present in the cluster which allows *FiFo* to survive single nodes failing as long as a minimum number of nodes is present.

The following nodes are running based on riak core:

* `Sniffle <../sniffle.html>`_
* `Snarl <../snarl.html>`_
* `Howl <../howl.html>`_

Being based on riak core it is recommended for ideal running operation to run at least five instances of this service on different hardware machines. This cluster is called a ring in riak terms (this has to do with the dynamo paper for those interested).

`Chunter <../chunter.html>`_
****************************

`Chunter <../chunter.html>`_ is a special case since it is directly tied to a hardware node which makes high availibility irrelevant.

Changing the cluster
********************

.. note::

	If the cluster is set up before the system goes into production there is not mucht to worry about. However there are situations where it is required to extend or shrink the cluster during production. Generally there is nothing that prevents extending or shrinking a cluster during production but it requires the user to perform an additional step.

To prevent interruptions it is nessessariy to disable the ``mdns`` on the nodes that you want to add or remove. Otherwise other services will think these nodes are valid parts of the cluster and therefore try to access them. To disable ``mdns`` the configuration option ``mdns.server = disabled`` needs to be added to the  `Sniffle Configuration File <../sniffle/configuration.html#mdns>`_, `Snarl Configuration File <../Snarl/configuration.html#mdns>`_ or `Howl Configuration File <../howl/configuration.html#mdns>`_ depending on what service is added or removed. This should be done before the services are first started or removed / leaves.

For the following section the existing server will be named ``sniffle@10.0.0.1`` and the new server will be named ``sniffle@10.0.0.2``, if multiple servers exist it does not matter which one is picked. Also keep in mind that for working with a `Howl <../howl.html>`_ or `Snarl <../snarl.html>`_ cluster the section before the ``@`` needs to be replaced with ``howl`` and ``snarl`` respectively.

New Cluster Node Preparation
****************************

Prepare a new *FiFo Node* as described in the `Installation <installation.html>`_ section. 

.. note::

	The first *Fifo Node* or existing *FiFo Cluster* will likely already contain an admin account so that the `last step of the Installation section <installation.html#initial-administrative-tasks>`_ is not needed. If additional admin accounts are created it will not fail the join operation. Clean up of redundancies can be performed within the UI or using the fifoadm command.

Adding a server
***************

To add a server the following steps need to be taken: 

- on the **existing server** check the server status with `ring status <../sniffle/administration.html#ring-status>`_ and `member status <../sniffle/administration.html#member-status>`_.
- on the **new server** join the **existing server** with the `join <../sniffle/administration.html#join>`_ command.
- on the **existing server** re check the server status with `ring status <../sniffle/administration.html#ring-status>`_ and `member status <../sniffle/administration.html#member-status>`_. commands.

.. seealso::

	Exact descriptions of all commands can be found in the `Sniffle Administration Guide <../sniffle/administration.html#cluster>`_, `Snarl Administration Guide <../Snarl/administration.html#cluster>`_ and `Howl Administration Guide <../Snarl/administration.html#cluster>`_.

Removing a server
*****************

Removing a server is as simple as adding one. To ensure it is not picked up again after it is removed the line ``mdns.server = disabled`` should be added to the configuration file of the server you want to remove.

- on the **existing server** check the server status with `ring status <../sniffle/administration.html#ring-status>`_ and `member status <../sniffle/administration.html#member-status>`_.
- on the **existing server** re check the server status with `ring status <../sniffle/administration.html#ring-status>`_ and `member status <../sniffle/administration.html#member-status>`_ commands.
- once the server is removed it is restarted and can be shut down using the ``svcadm disable sniffle`` command.
