.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
Administration
**************

The `Snarl <../snarl.html>`_ admin command is ``/opt/local/fifo-snarl/bin/snarl-admin`` but many commands can also be accessed via ``fifoadm`` command. Please keep in mind that ``fifoadm`` is not designed as an every day command but only as a last fall-back when commands are not available through the API.

General Management
##################

`Snarl <../snarl.html>`_ uses the SMF to manage it's running state so it is restarted in the case of crashes and booted accordingly on system start. `Snarl <../snarl.html>`_ can be enabled, disabled and restarted via: ``svcadm enable snarl``, ``svcadm disable snarl`` and ``svcadm restart snarl``

Updating
********
`Snarl <../snarl.html>`_ can be updated by three simple steps.

1. Installing the new package
*****************************

.. code-block:: bash

   pkgin -fy update
   pkgin -fy install fifo-snarl


2. Updating the config
**********************

After the newest package is installed, the config file should be checked for changes and edited if needed. The ``.example`` file will always contain the newest version of the config ``diff`` is a handy tool to see if some settings need to be added to the existing file.

.. code-block:: bash

   diff /opt/loca/fifo-snarl/etc/snarl.conf /opt/loca/fifo-snarl/etc/snarl.conf.example
   vi /opt/loca/fifo-snarl/etc/snarl.conf


3. Restarting the service
*************************

After the config is updated the service needs to be restarted. If `Snarl <../snarl.html>`_ is running in a clustered configuration and has more then ``N`` nodes it is often possible to do a rolling update by restarting one by one.

Cluster management
##################

snarl-admin join ``<nodename>@<ip>``
    Joins a `Snarl <../snarl.html>`_ cluster, please note that all data on the joining (not the joined) node is deleted.

snarl-admin leave
    Cleanly removes a node from the ring. This is helpful when nodes get moved and the cluster downsized

snarl-admin remove ``<nodename>@<ip>``
    Forcefully removes a node from the ring. This can be used after fatal node cluster.

snarl-admin member-status
    Lists the status of each node and the distribution of data over the ring nodes.

    ::

       ================================= Membership ==================================
       Status     Ring    Pending    Node
       -------------------------------------------------------------------------------
       valid     100.0%      --      'snarl@172.16.0.254'
       -------------------------------------------------------------------------------
       Valid:1 / Leaving:0 / Exiting:0 / Joining:0 / Down:0


snarl-admin ring-status
    Gives an extended report on the ring including hand-offs and downed nodes.

    ::

       ================================== Claimant ===================================
       Claimant:  'snarl@172.16.0.254'
       Status:     up
       Ring Ready: true

       ============================== Ownership Handoff ==============================
       No pending changes.

       ============================== Unreachable Nodes ==============================
       All nodes are up and reachable


snarl-admin status
    A simple command that returns the overall cluster status. It returns a proper return code and is useful for scripted rolling updates.

snarl-admin aae-status
    Gives a detailed status on the AAE status of the system.

Log Files
#########

*FiFo* uses extensive logging to make debugging issue and understanding behavior. The log files are located in ``/var/log/snarl/``. There are multiple log files with various severities.

debug.log
    By default the debug log is disabled, it is very verbose and should not be enabled in production systems to enable it uncomment the following line in the ``snarl.conf``

    ::

       log.debug.file = /var/log/snarl/debug.log


console.log
    This file contains logs of the level info and above, usually all interesting logs can be found here.

error.log
    This files contains errors, it usually should be mostly empty but please keep in mind that failing is not a uncommon practice to deal with unexpected behavior so sporadic entries might just be fine.

General tasks
#############

snarl-admin ``users|role`` add ``<realm>`` ``<name>``
    Adds a user or role, especially helpful when no users exist yet.

snarl-admin ``users|role`` grant ``<realm>`` ``<name>`` ``<permission>`` ``[<permission>]``
    Grants a user or role permissions. A permission can have multiple elements so: ``snarl-admin users grant user some special permissions`` would grant ``some->special->permissions`` to ``user``. ``...`` and ``_`` are special cases and not taken as strings but as the wildcards for the permission section where ``_`` means this level matches and ``...`` means everything below this level matches.

snarl-admin ``users`` passwd ``<realm>`` ``<username>`` ``<password>``
    Changes the password for a user.

snarl-admin ``users|roles|orgs|`` list ``<realm>``
    Lists all users, roles or organisations.

snarl-admin ``users|roles|orgss|`` delete ``<realm>`` ``<name>``
    Delets a given user, role or organisation.
