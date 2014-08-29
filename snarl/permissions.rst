.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

***********
Permissions
***********

Users
#####

User permissions start with ``users->`` followed by the UUID of the user. In addition to user specific permissions the following ``cloud`` global permission exist.

``cloud->users->list``
    Allows to list users, the ``get`` permission on each user determins which users are seen during the list.

``cloud->users->create``
    Allows the to create new users.

``users-><UUID>->get``
    Allows to see the user/read it's data.

``users-><UUID>->delete``
    Allows to delete the user.

``users-><UUID>->edit``
    Allows to change the user.

``users-><UUID>->passwd``
    Allows to change the password of the user.

``users-><UUID>->grant``
    Allows to grant a permission to the user.

``users-><UUID>->revoke``
    Allows to revoke a permission from the user.

``users-><UUID>->join``
    Allows to have the user join a role.

``users-><UUID>->leave``
    Allows to remove the user from a role.

Roles
#####

Role permissions start with ``roles->`` followed by the UUID of the role. In addition to user specific permissions the following ``cloud`` global permission exist.

``cloud->roles->list``
    Allows to list roles, the ``get`` permission on each role determins which users are seen during the list.

``cloud->roles->create``
    Allows the to create new roles.

``roles-><UUID>->get``
    Allows to see the role/read it's data.

``roles-><UUID>->delete``
    Allows to delete the role.

``roles-><UUID>->edit``
    Allows to change the role.

``roles-><UUID>->grant``
    Allows to grant a permission to the role.

``roles-><UUID>->revoke``
    Allows to revoke a permission from the role.

``roles-><UUID>->join``
    Allows to have a user join the role.

``roles-><UUID>->leave``
    Allows to removea user from the role.

Organisations
#############

Role Permissions start with ``orgs->`` followed by the UUID of the organisation. In addition to user specific permissions the following ``cloud`` global permission exist.

``cloud->orgs->list``
    Allows to list organisations, the ``get`` permission on each organisation determins which users are seen during the list.

``cloud->orgs->create``
    Allows the to create new organisations.

``orgs-><UUID>->get``
    Allows to see the organisation/read it's data.

``orgs-><UUID>->delete``
    Allows to delete the organisation.

``orgs-><UUID>->edit``
    Allows to change the organisaiton
