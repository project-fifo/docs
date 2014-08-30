.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
Administration
**************

The `Sniffle <../sniffle.html>`_ admin command is ``sniffle-admin`` but many commands can also be accessed via ``fifoadm`` command. Please keep in mind that ``fifoadm`` is not designed as an every day command but only as a last fallback when commands are not available through the API.

General managemnet
##################

`Sniffle <../sniffle.html>`_ uses the SMF to manage it's running state so it is restarted in the case of crashes and booted accordingly on system start. `Sniffle <../sniffle.html>`_ can be enabled, disabled and restarted via: ``svcadm enable sniffle``, ``svcadm disable sniffle`` and ``svcadm restart sniffle``

Updating
********

`Sniffle <../sniffle.html>`_ can be updated by three simple steps.

Installing the new package
**************************

.. code-block:: bash

   pkgin -fy update
   pkgin -fy install fifo-sniffle


Updating the config
*******************

After the newest package is installed the config file should be checked for changes and eddited if needed. The ``.example`` file will always contain the newest version of the config ``diff`` is a handy tool to see if some settings need to be added to the existing file.

.. code-block:: bash

   diff /opt/loca/fifo-sniffle/etc/sniffle.conf /opt/loca/fifo-sniffle/etc/sniffle.conf.example
   vi /opt/loca/fifo-sniffle/etc/sniffle.conf

Restarting the service
**********************

After the config is updated the service needs to be restarted. `Sniffle <../sniffle.html>`_ is running clustered and has more then ``N`` it is often possible to do a rolling update by restarting one by one.

Cluster management
******************

sniffle-admin join ``<nodename>@<ip>``
    Joins a sniffle cluster, please note that all data on the joining (not the joined) node is deleted.

sniffle-admin leave
    Cleanly removes a node from the ring. This is helpful when nodes get moved and the cluster downsized

sniffle-admin remove ``<nodename>@<ip>``
    Forcefully removes a node from the ring. This can be used after fatal node cluster.

sniffle-admin member-status
    Lists the status of each node and the distribution of data over the ring nodes.

    +-----------------------------------------------------------------+
    |                           Membership                            |
    +========+==========+=========+===================================+
    | Status | Ring     | Pending | Nodes                             |
    +--------+----------+---------+-----------------------------------+
    | valid  | 100.0%   |   --    | 'snarl@172.16.0.254'              |
    +--------+----------+---------+-----------+-----------------------+    
    |Valid:1 | Leaving:0|Exiting:0| Joining:0 | Down:0                |
    +--------+----------+---------+-----------+-----------------------+

sniffle-admin ring-status
    Gives a extended report ont he ring, including handoffs and downed nodes.

    +-------------------------------------------------------------------+
    |Claimant                                                           |
    +===========+=======================================================+
    |Claimant   |'snarl@172.16.0.254'                                   |
    +-----------+--------------------+----------------------------------+    
    |Status     | up                 |                                  |
    +-----------+--------------------+----------------------------------+
    |Ring Ready | true               |                                  |
    +-----------+--------------------+----------------------------------+ 
    
    +-------------------------------------------------------------------+
    | Ownership handoff                                                 |
    +===================================================================+
    | No pending changes.                                               |
    +-------------------------------------------------------------------+
    
    +-------------------------------------------------------------------+
    | Unreachable Nodes                                                 |
    +===================================================================+
    | All nodes are up and reachable                                    |
    +-------------------------------------------------------------------+

sniffle-admin status
    A simple command that returns the overall cluster status, it returns a propper return code and is useful for scripted rolling updates.


sniffle-admin aae-status
    Gives a detailed status on the AAE status of the system.

Log Files
#########

*FiFo* uses extensive logging to make debugging issue and understanding behaviour. The log files are located in ``/var/log/sniffle/``. There are multiple log files with various severities.


debug.log
    By default the debug log is disabled, it is very verbose and should not be enabled in production systems to enable it uncomment the followig line in the ``sniffle.conf``

    ..

        log.debug.file = /var/log/sniffle/debug.log

console.log
    This file contains logs of the level info and above, usually all interesting logs can be found here.

error.log
    This files contains errors, it usually should be mostly empty but please keep in mind that failing is not a uncommon practice to deal with unexpected behavuiour so sporadic entries might just be fine.

General tasks
#############

sniffle-admin hypervisors <subcommand>
    * list - lists all avaiable hypervisors
    * delete ``<uuid>`` - removes a hypervisor

sniffle-admin vms
    * list - lists all VM's
    * delete ``<uuid>`` - deletes a VM

sniffle-admin packages
    * list - lists all Packages
    * delete ``<uuid>`` - deletes a Package

sniffle-admin datasets
    * list - lists all Datasets
    * delete ``<uuid>`` - deletes a Dataset

