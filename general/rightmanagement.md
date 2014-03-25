---
layout: default
category: general
section: general/rightmanagement
title: Rightmanagement
---
# Rightmanagement

## Permissions

<p class="bs-callout bs-callout-info">
Permissions can be given to both users and groups, however it best practice to concentrate on using groups to organize permissions.
</p>

FiFo's permission system uses a nested tree to represent it's permissions going from the general to the sepecific. Generally the second level is the UUID of the element with the exeption of the `cloud` tree.

```
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
`--groups
|   |
|   `...
```

For simplicity permissions are represented by the path of the permission joined by `->`. There are two special permissions, `...` which represents the "everything below this level" and `_` to everything at this level.

So translating the tree above this would look like:

- `vms->vm1->get` - can see VM `vm1`
- `vms->vm1->stop` - can stop VM `vm1`
- `vms->vm2->_` - can do everything to `vm2`
- `users->_->get` - can see every user
- `groups->...` - can do everything with all groups

In the pecific examples `<UUID>` is treated as a placeholder for a element or of cause can be `_` for all elements.

For the specific permissions on [users](/snarl/permissions.html#users), [organisations](/snarl/permissions.html#organisations) and [groups](/snarl/permissions.html#groups) please see the [Snarl Permissions]() section. For [vms](/sniffle/permissions.html#vms), [hypervisors](/sniffle/permissions.html#hypervisors), [datasets](/sniffle/permissions.html#datasets), [dtrace](/sniffle/permissions.html#dtrace), [ipranges](/sniffle/permissions.html#ipranges), [networks](/sniffle/permissions.html#networks) and [packages](/sniffle/permissions.html#packages) see the [Sniffle Permissions](/sniffle/permissions.html) section. For [channels](/howl/permissions.html#channels) see the [Howl Permissions](/howl/permissions.html) section.

## Organisations

FiFo abstracts the concept of a *Organisation* (Customer, Client) away from the user. This means one *Organistaiton* can have multiple users, and a *User* (for example an admin user) can belong to multiple *Organisations*. However, belonging to an *Organisation* does not grant a *User* any permissions, this is handled by assigning *Groups*. A *User* can select one of the *Organisations* he blongs to *active* meaning he acts for the *Organisation* and triggers certain events.

### Triggers
Triggers are where Organisation get their power from. They are miniature scripts that get executed when certain events occour.

#### Trigger events
A trigger basically has two elements, the event and the uuid of the element that triggered it.

##### vm_create
Triggered when a VM is created, the element passed is the UUID of the new VM.

##### dataset_create
Triggered when a dataset created, the element passed is the UUID of the new dataset.

##### user_create
Triggered when a user is created, the element passed is the UUID of the new ser.

#### Trigger actions
Actions are what happens when a trigger is created, the special term `$` in the trigger will replaced with the element provided by the event.

##### group_grant
Grants a triggers automatically grant a permission to a *Group*. The elements provided for this tirgger are:

* `target` - the uuid of the *Group* to grant to.
* `permission` - the permission to grant where `$` marks the uuid of the element.

##### user_grant
Grants a triggers automatically grant a permission to a *User*. The elements provided for this tirgger are:

* `target` - the uuid of the *User* to grant to.
* `permission` - the permission to grant where `$` marks the uuid of the element.

##### join_group
Joins a new *User* to a *Group*. This should not be used vm or dataset events.

* `target` - the uuid of the *Group* to grant to.

##### join_group
Joins a new *User* to a *Organisation*. This should not be used vm or dataset events.

* `target` - the uuid of the *Group* to grant to.

## Example

### Groups<a id="group-example"></a>
This is an example for a general Users groups that covers the basic permissions required by each user.

<p class="bs-callout bs-callout-warning">
Please note the `channels->_->join` permission, this is to work around the limitations of how howl checks permissions at this moment, however channels are read only and require knowledge about the VM's UUID to join. This can be skipped but will not allow to see metrics for VM's that permissions are received via Organisation grant triggers. This is mea
</p>

```
channels->_->join
cloud->cloud->status
cloud->datasets->list
cloud->dtraces->list
cloud->groups->list
cloud->hypervisors->list
cloud->ipranges->list
cloud->networks->list
cloud->orgs->list
cloud->packages->list
cloud->users->list
cloud->vms->list
groups->Users->get
hypervisors->_->create
hypervisors->_->get
packages->_->get
datasets->_->get
```
<p class="bs-callout bs-callout-info">
This group assumes all users are allowed ot use all packages and datasets (`packages->_->get` and `datasets->_->get`) if this is not wanted the permissions must be set on a different level and more respective.
</p>

<p class="bs-callout bs-callout-info">
This is meanted to be used in connection with the <a href="/general/rightmanagement.html#org-example">Example Org</a> to give users the right to create VM's otherwise the following permission needs to be added to grant all users permission to create VMs: `cloud->vms->create`.
</p>


### Organisation<a id="org-example"></a>
Here is a set of rules that represents a good default organisation with three assiciarted groups. This is meant to be used in combination with a general [Users Group](/general/rightmanagement.html#org-example)

#### Admins
Administrative users that have full power over resources of the Organistation.

##### Basic permissions
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

##### Triggers

```
channels->$->join
datasets->$->...
users->$->...
vms->$->...
```

#### Users
Normal users that can can see, start, stop, restart VM's but not create or delete them.

##### Basic permissions
Those are the basic permissions the Users group starts off with.

```
groups-> <RO UUID> ->get
groups-> <Admins UUID> ->get
groups-> <Users UUID> ->get
ipranges-> <Org IP-Range> ->get
networks-> <Org Network> ->get
```

##### Triggers

```
channels->$->join
datasets->$->get
vms->$->get
vms->$->reboot
vms->$->start
vms->$->stop
```

#### RO
Read Only users that can see VM's but not work with them.

##### Basic permissions
Those are the basic permissions the RO group starts off with.

```
groups-> <RO UUID> ->get
groups-> <Admins UUID> ->get
groups-> <Users UUID> ->get
ipranges-> <Org IP-Range> ->get
networks-> <Org Network> ->get
```

##### Triggers

```
channels->$->join
datasets->$->get
vms->$->get
```
