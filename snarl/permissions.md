---
layout: default
category: snarl
section: snarl/permissions
title: Permissions
---
# Snarl Permissions

## Users<a id="users"></a>
User permissions start with `users->` followed by the UUID of the user. In addition to user specific permissions the following `cloud` global permission exist.

### `cloud->users->list`
Allows to list users, the `get` permission on each user determins which users are seen during the list.

### `cloud->users->create`
Allows the to create new users.

### `users-><UUID>->get`
Allows to see the user/read it's data.

### `users-><UUID>->delete`
Allows to delete the user.

### `users-><UUID>->edit`
Allows to change the user.

### `users-><UUID>->passwd`
Allows to change the password of the user.

### `users-><UUID>->grant`
Allows to grant a permission to the user.

### `users-><UUID>->revoke`
Allows to revoke a permission from the user.

### `users-><UUID>->join`
Allows to have the user join a group.

### `users-><UUID>->leave`
Allows to remove the user from a group.

## Groups<a id="groups"></a>
Group permissions start with `groups->` followed by the UUID of the group. In addition to user specific permissions the following `cloud` global permission exist.

### `cloud->groups->list`
Allows to list groups, the `get` permission on each group determins which users are seen during the list.

### `cloud->groups->create`
Allows the to create new groups.

### `groups-><UUID>->get`
Allows to see the group/read it's data.

### `groups-><UUID>->delete`
Allows to delete the group.

### `groups-><UUID>->edit`
Allows to change the group.

### `groups-><UUID>->grant`
Allows to grant a permission to the group.

### `groups-><UUID>->revoke`
Allows to revoke a permission from the group.

### `groups-><UUID>->join`
Allows to have a user join the group.

### `groups-><UUID>->leave`
Allows to removea user from the group.

## Organisations<a id="organisations"></a>
Group Permissions start with `orgs->` followed by the UUID of the organisation. In addition to user specific permissions the following `cloud` global permission exist.

### `cloud->orgs->list`
Allows to list organisations, the `get` permission on each organisation determins which users are seen during the list.

### `cloud->orgs->create`
Allows the to create new organisations.

### `orgs-><UUID>->get`
Allows to see the organisation/read it's data.

### `orgs-><UUID>->delete`
Allows to delete the organisation.

### `orgs-><UUID>->edit`
Allows to change the organisaiton
