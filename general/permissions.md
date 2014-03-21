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
