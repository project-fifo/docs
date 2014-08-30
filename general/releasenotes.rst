.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*************
Release Notes
*************

0.6.0
#####

* Moved all datastrucutres to CRDT's

  This will bring a huge improvment in stability and the capability to deal with network failures and partitions. As part of this also the conversion to JSON for the API was pushed away from the core components towards the API endpoints.

* Supporting multiple realms for Snarl

  * This allows to use the same Snarl for multiple useses.

* Metric support for Snarl sending data to `DalmatierDB <https://dalmatiner.io>`_
* Added EQC (Erlang Quick Check) tests for datatypes and some components.
* Logging of VM creation is vastly improved to allow better determination of problems.
* Improvements of the cold migration workflow in the ui.
* Numerous UI and UX improvements
* Fixes of edgecasees and minor bugs
* Updated the documentation to use sphynx and `Read the docs <http://readthedocs.org>`_
* Updated to the latest riak_core
* Updated to cowboy and ranch 1.0.0


0.4.5
#####

* Cloud topology

  * Clusters - ensuring VMs are not placed on the same hypervisor.
  * Stacks - keeping VMs placed as close as possible.
  * Hypervisor topology - describing the layout of the cloud to *FiFo*.

* Caching for `Wiggle <../wiggle.html>`_.
* Dataset upload over the *Fifo* commandline client.
* Dry run VM creation to ensure validity of Package/Dataset/Network combination prior to VM creation.
* As usual fixes and improvments.

0.4.4
#####

* An important update to the Active Anti Entropy engine.
* Commands to perform database updates.
* Diagnostic commands (sniffle-diag, snarl-diag, ...).
* The commands sniffle, snarl, sniffle-admin, and snarl-admin are now put into /opt/local/sbin.
* A chineese translation of the UI.
* More improvements on the config files.
* Minor fixes and improvements.

0.4.3
#####

This is the so far biggest release of *Project-FiFo*: aside from the usual improvements and fixes we've added a good number of new and exciting features!

.. attention::

   With 0.4.3 we have made the decision to tune defaults towards a 1 node installation instead of a full-blown multi node setup.

   The reasoning for this is simple: one node installations are usually done by people trying out the project for the first time, mostly without a firm gasp of what it is and what they are wanting to do with it. Those installations are usually the same across the board so having a default that 'fits all' is much easier. Also scaling up is easier then scaling down once the installation grows.

   If you are using *FiFo* in production it is more important then ever to plan ahead and understand the environment in which you are running it to pick some sane defaults of your use case.


Configuration
*************

New configuration files
***********************

Starting with this release we use `Cuttlefish <https://github.com/basho/cuttlefish>`_. This means configurations files are now much easier to maintain and more ops friendly adopting a simple ``key = value`` syntax for settings instead of Erlang syntax that fewer people are familiar with. This change however requires some additional work as described in the `upgrade instructions <upgrade.html#id3>`_.

Global configuration
********************

With some of the settings (like the s3 or yubikey settings) globally valid it makes sense to not require them to be configured for each node. Instead they are now globally configurable with ``sniffle-admin config`` and ``snarl-admin config``. 

.. seealso::

  For detailed information see: `Sniffle Configuration <../sniffle/configuration.html#global>`_ and `Snarl Configuration <../sniffle/configuration.html#global>`_ section.

Reserved memory
***************

It is now possible to reserve memory on the hypervisor. This makes managing things like ARC memory easier while guaranteeing some memory for the global zone. This can be done in the [`Chunter Configuration <../chunter/configuration.html#file>`_.

`Improved chunter installation <../chunter/installation.html>`_
***************************************************************

Upon the first install chunter tries to guess the IP configuration of the system to determine what interface to use to talk to the other services. This now checks for a ``fifo0`` interface it will favor over the ``admin`` interface when present allowing for easier setup in large environments.

`Snarl <../snarl.html>`_
*************************

Multi-Datacenter support
************************

Starting with this release Snarl allows for synchronizing logins, roles, organizations and tokens over multiple data centers. This makes it possible to combine multiple data centers into one logical FiFo unit while still keeping the datacenter operation tasks separated to prevent dependencies and possible downtime. This requires `setting up synchronization endpoints <../snarl/configuration.html#multidc>`_ between involved services.

Yubikey support
***************

A cloud requires security and while a normal username/password login is nice and good it sometimes isn't enough. Yubikeys are a very simple answer to multi factor authentication. Yubikey support is now a feature built in `Snarl <../snarl.html>`_. The configuration is rather easy and can be done via the new `global configuration <../snarl/configuration.html#yubikey>`_.

`Sniffle <../sniffle.html>`_
****************************

LeoFS (S3) integration
**********************

*FiFo* can now integrate with `LeoFS <http://leofs.org>`_ (or probably any other S3 API) to separate storage from management. With S3 storage enabled datasets get stored in S3 instead of Sniffle directly improving scalability and performance. The needed configuration can be handled via the global [sniffle configuration](/sniffle/configuration.html#global) and is automatically synchronized between all hosts.

Backups
*******

.. warning::

   Please be warned that backups are still a experimental feature and while they have not shown any failures during the time 0.4.3 was in development mode they should not be relied on as the only copy of sensitive data.


In addition to storing datasets the introduction of a external storage made it possible to introduce a entirely new feature, backups. Backups are the logical conclusion from the already existing snapshots for VM's just instead of being stored on the hypervisor with the VM they are relocated to a separate storage system.

The separation from the VM allows for a number of new features like reverting backups ups without loosing newer versions, recovering VM's after total hypervisor loss, and last but not least moving VM's from one hypervisor to another using the backup to restore them.

Datasets
********

There are new API calls for importing and exporting datasets, making it easy to push custom datasets to fifo or downloading datasets form *FiFo* to publish to places like `datasetsat <http://www.datasets.at>`_. However client support for those commands is still pending.

VM Creation
***********

The logic behind the creation process of VM's have been significantly altered and improved, the new logic prevents race conditions when creating multiple VM's in parallel, overloading of ``vmadm`` by queuing creates on a per hypervisor level and potential issues in the case of a network split. The new code has been tested with **1:300** ratio of hypervisors/creation-requests and performs safely under this conditions.


Services
********

*FiFo* now allows managing services (as in SMF services) over the FiFo API this means it's easy to monitor the state of a service or change it if needed. This is possible for both the Global Zone and SmartOS VMs.

Hypervisor updates
******************

`Sniffle <../sniffle.html>`_ now contains code to trigger `Chunter <../chunter.html>`_ updates. This simplifies the management of large amounts of hypervisors since it requires just one command to update all of them instead of doing it on each seperately. It still is possible to trigger single hypervisors either locally or from *FiFo* directly.

The related commands in the FiFo Zone are: ``fifoadm hypervisors update`` and ``fifoadm hypervisors update <hypervisor>``.

General
*******

Active Anti Entropy(AAE)
************************

.. attention::

   AAE is disabled by default since it does not make sense to use it with less then two nodes.


We back-ported the code used in `riak for Active Anti Entropy <https://basho.com/tag/active-anti-entropy/>`_ to increase the consistency of data within FiFo, this minimizes the chance of less frequently accessed data.

Memory consumption
******************

.. attention::

   The tuning done to reduce memory consumption reflects directly a reduced performance for larger installations. If you are running more then one node please be sure to have a look at configuration values as the number of vnodes or the mmap_size settings for `Sniffle <../sniffle.html>`_, `Snarl <../snarl.html>`_ and `Howl <../howl.html>`_.

With a growing number services running inside of *FiFo* the amount of memory it requires grows steadily. With the 0.4.3 release we've taking steps to significantly reduce the memory requirement to a bearable level. All the changes done are covered by configuration values and tuned for a 1 node installation to make starting off easy. It is worth investing some time tweaking the settings.

UI
**

`Jingles <../jingles.html>`_
****************************

The entire user interface has been reworked providing a lighter and more pleasant look while improving usability and providing a more logical structure of the components.

Documentation
*************

Along with the rework of the interface the entire documentation has been reworked. It is now more detailed in content while easier to access.
