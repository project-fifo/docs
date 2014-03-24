---
layout: default
category: general
section: general/orgs
title: Permissions
---
# Organisations

FiFo abstracts the concept of a *Organisation* (Customer, Client) away from the user. This means one *Organistaiton* can have multiple users, and a *User* (for example an admin user) can belong to multiple *Organisations*. However, belonging to an *Organisation* does not grant a *User* any permissions, this is handled by assigning *Groups*. A *User* can select one of the *Organisations* he blongs to *active* meaning he acts for the *Organisation* and triggers certain events.

## Triggers
Triggers are where Organisation get their power from. They are miniature scripts that get executed when certain events occour.

### Trigger events
A trigger basically has two elements, the event and the uuid of the element that triggered it.

#### vm_create
Triggered when a VM is created, the element passed is the UUID of the new VM.

#### dataset_create
Triggered when a dataset created, the element passed is the UUID of the new dataset.

#### user_create
Triggered when a user is created, the element passed is the UUID of the new ser.

### Trigger actions
Actions are what happens when a trigger is created, the special term `$` in the trigger will replaced with the element provided by the event.

#### group_grant
Grants a triggers automatically grant a permission to a *Group*. The elements provided for this tirgger are:

* `target` - the uuid of the *Group* to grant to.
* `permission` - the permission to grant where `$` marks the uuid of the element.

#### user_grant
Grants a triggers automatically grant a permission to a *User*. The elements provided for this tirgger are:

* `target` - the uuid of the *User* to grant to.
* `permission` - the permission to grant where `$` marks the uuid of the element.

#### join_group
Joins a new *User* to a *Group*. This should not be used vm or dataset events.

* `target` - the uuid of the *Group* to grant to.

#### join_group
Joins a new *User* to a *Organisation*. This should not be used vm or dataset events.

* `target` - the uuid of the *Group* to grant to.

## Example<a id="example"></a>
Here is a set of rules that represents a good default organisation with three assiciarted groups. This is meant to be used in combination with a general [Users Group](/general/permissions.html#example)

### Admins
Administrative users that have full power over resources of the Organistation.

#### Basic permissions
Those are the basic permissions the Admin group starts off with.

```
cloud->users->create
cloud->vms->create
groups-> <RO UUID> ->...
groups-> <Admins UUID> ->...
groups-> <Users UUID> ->...
ipranges-> <Org IP-Range> ->get
networks-> <Org Network> ->get
orgs-> <Org UUID> ->...
```

#### Triggers

```
channels->$->join
datasets->$->...
users->$->...
vms->$->...
```

### Users
Normal users that can can see, start, stop, restart VM's but not create or delete them.

#### Basic permissions
Those are the basic permissions the Users group starts off with.

```
groups-> <RO UUID> ->get
groups-> <Admins UUID> ->get
groups-> <Users UUID> ->get
ipranges-> <Org IP-Range> ->get
networks-> <Org Network> ->get
```

#### Triggers

```
channels->$->join
datasets->$->get
vms->$->get
vms->$->reboot
vms->$->start
vms->$->stop
```

### RO
Read Only users that can see VM's but not work with them.

#### Basic permissions
Those are the basic permissions the RO group starts off with.

```
groups-> <RO UUID> ->get
groups-> <Admins UUID> ->get
groups-> <Users UUID> ->get
ipranges-> <Org IP-Range> ->get
networks-> <Org Network> ->get
```

#### Triggers

```
channels->$->join
datasets->$->get
vms->$->get
```
