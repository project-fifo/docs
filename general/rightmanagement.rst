.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

***************
Rightmanagement
***************

Permissions
===========

.. note::

   Permissions can be given to both users and roles, however it best practice to concentrate on using roles to organize permissions.


*FiFo*'s permission system uses a nested tree to represent its permissions going from the general to the sepecific. Generally the second level is the UUID of the element with the exeption of the ``cloud`` tree.

  ::

   --vms
   |   |
   |   `vm1
   |   |    |
   |   |    `get
   |   |    |
   |   |    `stop
   |   |
   |   `vm2
   |       |
   |       `_
   |
   `--users
   |   |
   |   `_
   |       |
   |       `get
   |
   `--roles
   |   |
   |   `...


For simplicity permissions are represented by the path of the permission joined by ``->``. There are two special permissions, ``...`` which represents the "everything below this level" and ``_`` to everything at this level.

So the ``cloud`` tree can be interpreted like this:

- ``vms->vm1->get`` - can see VM ``vm1``
- ``vms->vm1->stop`` - can stop VM ``vm1``
- ``vms->vm2->_`` - can do everything to ``vm2``
- ``users->_->get`` - can see every user
- ``roles->...`` - can do everything with all roles

In the specific examples ``<UUID>`` is treated as a placeholder for a element or of cause can be ``_`` for all elements.

.. seealso::
  For the specific permissions on `users <../snarl/permissions.html#users>`_, `roles <../snarl/permissions.html#roles>`_ and `organisations <../snarl/permissions.html#organisations>`_ please see the `Snarl Permissions <../snarl/permissions.html>`_ section. For `VMs <../sniffle/permissions.html#vms>`_, `hypervisors <../sniffle/permissions.html#hypervisors>`_, `datasets <../sniffle/permissions.html#datasets>`_, `dtrace <../sniffle/permissions.html#dtrace>`_, `ipranges <../sniffle/permissions.html#ipranges>`_, `networks <../sniffle/permissions.html#networks>`_ and `packages <../sniffle/permissions.html#packages>`_ see the `Sniffle Permissions <../sniffle/permissions.html>`_ section. For `channels <../howls/permissions.html#channels>`_ see the `Howl Permissions <../howl/permissions.html>`_ section.

Organisations
=============

*FiFo* abstracts the concept of an *Organisation* (Customer, Client) away from the user. This means one *Organistaiton* can have multiple users, and a *User* (for example an admin user) can belong to multiple *Organisations*. However, belonging to an *Organisation* does not grant a *User* any permissions, this is handled by assigning *Roles*. A *User* can select one of the *Organisations* he blongs to *active* meaning he acts for the *Organisation* and triggers certain events.

Triggers
--------

*Triggers* are where *Organisation* get their power from. They are miniature scripts that get executed when certain events occour.

Trigger events
``````````````

A trigger basically has two elements, the event and the uuid of the element that triggered it.

vm_create
    Triggered when a VM is created, the element passed is the UUID of the new VM.

dataset_create
    Triggered when a dataset created, the element passed is the UUID of the new dataset.

user_create
    Triggered when a user is created, the element passed is the UUID of the new ser.

Trigger actions
```````````````

Actions are what happens when a trigger is created, the special term ``$`` in the trigger will replaced with the element provided by the event.

role_grant
    Grants a triggers automatically grant a permission to a *Role*. The elements provided for this tirgger are:

    target
        the uuid of the *Role* to grant to.

    permission
        the permission to grant where ``$`` marks the uuid of the element.

user_grant
    Grants a triggers automatically grant a permission to a *User*. The elements provided for this tirgger are:

        target
            the uuid of the *User* to grant to.

        permission
            the permission to grant where ``$`` marks the uuid of the element.

join_role
    Joins a new *User* to a *Role*. This should not be used vm or dataset events.

    target
        the uuid of the *Role* to join.

join_org
    Joins a new *User* to a *Organisation*. This should not be used for vm or dataset events.

    target
        the uuid of the *Organisation* to join.

Example
=======

Roles
-----
This is an example for a general Users roles that covers the basic permissions required by each user.

.. warning::

   Please note the ``channels->_->join`` permission. This permission exists to work around limitations in the way howl checks permissions. However channels are read only and require knowledge about the VMs UUID to join. This can be skipped but will not allow to see metrics for VMs that permissions are received via Organisation grant triggers.


::

   channels->_->join
   cloud->cloud->status
   cloud->datasets->list
   cloud->dtraces->list
   cloud->roles->list
   cloud->hypervisors->list
   cloud->ipranges->list
   cloud->networks->list
   cloud->orgs->list
   cloud->packages->list
   cloud->users->list
   cloud->vms->list
   roles->Users->get
   hypervisors->_->create
   hypervisors->_->get
   packages->_->get
   datasets->_->get

.. note::

   This role assumes all users are allowed to use all packages and datasets (``packages->_->get`` and ``datasets->_->get``) if this is not wanted the permissions must be set on a different level and more respective.

.. note::

   This is meant to be used in connection with the <a href="/general/rightmanagement.html#org-example">Example Org</a> to give users the right to create VMs. Otherwise the following permission needs to be added to grant all users permission to create VMs: ``cloud->vms->create``.


Organisation
------------

Here is a set of rules that represents a good default organisation with three assiciarted roles. This is meant to be used in combination with a general User Role.

Admins
``````


Administrative users that have full power over resources of the Organistation.

Basic permissions
'''''''''''''''''

Those are the basic permissions the Admin role starts off with.

::

   cloud->users->create
   cloud->vms->create
   roles-> <RO UUID> ->...
   roles-> <Admins UUID> ->...
   roles-> <Users UUID> ->...
   ipranges-> <Org IP-Range> ->get
   networks-> <Org Network> ->get
   orgs-> <Org UUID> ->...


Triggers
''''''''

::

   channels->$->join
   datasets->$->...
   users->$->...
   vms->$->...


Users
`````

Normal users can see, start, restart and stop VMs but are not allowed to create or delete them.

Basic permissions
''''''''''''''''''

Those are the basic permissions the Users role starts off with.

::

   roles-> <RO UUID> ->get
   roles-> <Admins UUID> ->get
   roles-> <Users UUID> ->get
   ipranges-> <Org IP-Range> ->get
   networks-> <Org Network> ->get


Triggers
''''''''

::

   channels->$->join
   datasets->$->get
   vms->$->get
   vms->$->reboot
   vms->$->start
   vms->$->stop


RO
``

Read Only users that can see VMs but are not allowed to work with them.

Basic permissions
'''''''''''''''''

Those are the basic permissions the RO role starts off with.

::
   roles-> <RO UUID> ->get
   roles-> <Admins UUID> ->get
   roles-> <Users UUID> ->get
   ipranges-> <Org IP-Range> ->get
   networks-> <Org Network> ->get


Triggers
''''''''

::

   channels->$->join
   datasets->$->get
   vms->$->get
