.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

************
API - Roles
************

.. http:get:: /roles

   Lists all roles.

   **Related permissions**

   cloud -> roles -> list 


.. http:post:: /roles

   Creates a new role.

   **Related permissions**

   cloud -> roles -> create


.. http:get:: /roles/(uuid:role)

   Returns role with given *uuid*.

   **Related permissions**

   roles -> ID -> get


.. http:delete:: /roles/(uuid:roles)

   Deletes role with given *uuid*.

   **Related permissions**

   roles -> ID -> delete
      

.. http:get:: /roles/(uuid:role)/permissions

   Lists permissions for role with given *uuid*.

   **Related permissions**

   roles -> ID -> get


.. http:put:: /roles/(uuid:role)/permissions/<permission>

   Grants <permission> for role with given *uuid*.

   **Related permissions**

   * roles -> ID -> grant
   * permissions -> PERMISSION -> grant


.. http:delete:: /roles/(uuid:role)/permissions/<permission>

   Revokes <permission> for role with given *uuid*.

   **Related permissions**

   * users -> ID -> grant
   * permissions -> PERMISSIONS -> revoke


.. http:put:: /roles/(uuid:role)/metadata[/...]

   Sets a metadata key for role with given *uuid*.

   **Related permissions**

   roles -> UUID -> edit


.. http:delete:: /roles/(uuid:role)/metadata/...

   Removes a key from the metadata for role with given *uuid*.

   **Related permissions**

   roles -> UUID -> edit
