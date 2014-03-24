---
layout: default
category: general
section: general/permissions
title: Permissions
---
# Permissions

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


## Example<a id="example"></a>
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
This is meanted to be used in connection with the [Example Org](/general/permissions.html#example) to give users the right to create VM's otherwise the following permission needs to be added to grant all users permission to create VMs: `cloud->vms->create`.
</p>
