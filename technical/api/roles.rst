.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

************
API - Roles
************

.. http:get:: /roles

   Lists all roles.

   **Related permissions**

     cloud -> roles -> list 

   **Example request**:

      .. sourcecode:: http
  
        GET /roles HTTP/1.1
        host: cloud.project-fifo.net
        accept: applicaiton/json
        x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 200 OK
       vary: Accept
       content-type: application/json
       x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16
  
       ["b7c658e0-2ddb-46dd-8973-4a59ffc9957e"]


   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :reqheader x-full-list: true - to get a full list instead of UUIDs
   :reqheader x-full-fields: fields to include in the full list - please see: :http:get:`/roles/(uuid:role)`
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session
   
   :status 200: the roles list is returned
   :status 403: user is not authoriyed
   :status 503: one or more subsystems could not be reached

.. http:post:: /roles

   Creates a new role.

   **Related permissions**

      cloud -> roles -> create


.. http:get:: /roles/(uuid:role)

   Returns role with given *uuid*.

   **Related permissions**

      roles -> ID -> get

   **Example request**:

      .. sourcecode:: http
  
       GET /roles/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
       host: cloud.project-fifo.net
       accept: applicaiton/json
       x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 200 OK
       vary: Accept
       content-type: application/json
       x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16
  
       {
        "uuid": "b7c658e0-2ddb-46dd-8973-4a59ffc9957e",
        "name": "Administrators",
        "permissions": [["..."]],
        "metadata": {}
       }

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the role information is returned
   :status 403: user is not authoriyed
   :status 404: the role was not found
   :status 503: one or more subsystems could not be reached

   :>json string uuid: UUID of the role
   :>json string name: name of the role
   :>json array permissions: list of permissions that are associated with the role
   :>json object metadata: metadata associated with the role

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
