.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

***********
API - Users
***********

.. http:get:: /users

   Lists all users.

   **Related permissions**

      users -> ID -> list 

   **Example request**:

      .. sourcecode:: http
  
        GET /users HTTP/1.1
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
   :reqheader x-full-fields: fields to include in the full list - please see: :http:get:`/users/(uuid:user)`
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session
   
   :status 200: the user list is returned
   :status 403: user is not authoriyed
   :status 503: one or more subsystems could not be reached








.. http:post:: /users

   Creates a new user.

   **Related permissions**

      users -> ID -> create

.. todo::

  Example Requests & Responses still missing.








.. http:get:: /users/(uuid:user)

   Returns user with given *uuid*.

   **Related permissions**

      users -> ID -> get

   **Example request**:

    .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
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
      "name": "admin",
      "roles": [],
      "org": "",
      "orgs": [],
      "permissions": [["..."]],
      "keys": {"key-id": "ssh-rsa ..."},
      "yubikeys": [],
      "metadata": {}
     }


   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the user information is returned
   :status 404: the user was not found
   :status 403: user is not authoriyed
   :status 503: one or more subsystems could not be reached

   :>json string uuid: UUID of the user that is logged in
   :>json string name: name of the user that is logged in
   :>json array roles: list of role-UUIDs the user is a member of
   :>json string org: UUID of the currently active organization of the user
   :>json array orgs: list of org-uuid the user is member of
   :>json array permissions: list of permissions the user is granted
   :>json object keys: SSH public keys registered for the user
   :>json array yubikeys: YubiKey ID's for the user
   :>json object metadata: metadata associated with the user








.. http:put:: /users/(uuid:user)

   Changes password for user with given *uuid*.

   **Related permissions**

      users -> ID -> passwd

.. todo::
    
  Example Requests & Responses still missing.








.. http:delete:: /users/(uuid:user)

   Deletes user with given *uuid*.

   **Related permissions**

    users -> ID -> delete

   **Example request**:

      .. sourcecode:: http
  
       DELETE /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the user was successfully deleted
   :status 404: the user was not found
   :status 503: one or more subsystems could not be reached







      
.. http:get:: /users/(uuid:user)/permissions

   Lists permissions for user with given *uuid*.

   **Related permissions**

     users -> ID -> get

   **Example request**:

    .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/permissions HTTP/1.1
     host: cloud.project-fifo.net
     accept: applicaiton/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

    .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

      [["..."]]
     

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the user information is returned
   :status 404: the user was not found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json array permissions: list of permissions the user is granted
 









.. http:put:: /users/(uuid:user)/permissions/<permission>

   Grants <permission> to user with given *uuid*.

   **Related permissions**

     * users -> ID -> grant
     * permissions -> PERMISSIONS -> grant

.. todo::
    
  Example Requests & Responses still missing.








.. http:delete:: /users/(uuid:user)/permissions/<permission>

   Revokes <permission> for user with given *uuid*.

   **Related permissions**

      * users -> ID -> revoke
      * permissions -> PERMISSION -> revoke

   **Example request**:

      .. sourcecode:: http
  
       DELETE /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/permissions/users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/... HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the permission was successfully revoked from the user
   :status 404: the permission was not found for that user
   :status 503: one or more subsystems could not be reached








.. http:get:: /users/(uuid:user)/roles

   Lists roles for user with given *uuid*.

   **Related permissions**

      users -> ID -> get

   **Example request**:

    .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/roles HTTP/1.1
     host: cloud.project-fifo.net
     accept: applicaiton/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

    .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

      []

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: user's roles are returned
   :status 404: no roles were found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json array roles: list of roles the user is part of
      







.. http:put:: /users/(uuid:user)/roles/(uuid:role)

   Joins user with given *uuid* to role with given *uuid*.

   **Related permissions**

      * users -> ID -> join
      * roles -> ID -> join

.. todo::
    
  Example Requests & Responses still missing.


.. http:delete:: /users/(uuid:user)/roles/(uuid:role) 

   Deletes role with given *uuid* from user with given *uuid*.

   **Related permissions**

      * users -> UUID -> edit
      * roles -> ID -> edit

   **Example request**:

      .. sourcecode:: http
  
       DELETE /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/roles/c7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the role was successfully deleted for the user
   :status 404: the role was not found for the user
   :status 503: one or more subsystems could not be reached










.. http:get:: /users/(uuid:user)/keys

   Lists all install keys for user with given *uuid*.

   **Related permissions**

      users -> UUID -> get

   **Example request**:

    .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/keys HTTP/1.1
     host: cloud.project-fifo.net
     accept: applicaiton/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16
   
   **Example response**:

    .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16


      {"key-id": "ssh-rsa ..."}

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the user's keys are returned
   :status 404: the user was not found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json object keys: list of keys the user has access to










.. http:put:: /users/(uuid:user)/keys

   Adds a new SSH key to user with given *uuid*.

   **Related permissions**

      users -> UUID -> edit

.. todo::
    
  Example Requests & Responses still missing.








.. http:delete:: /users/(uuid:user)/keys/(uuid:key)

   Deltes key with given *uuid* for user with given *uuid*.

   **Related permissions**

      users -> UUID -> edit

   **Example request**:

      .. sourcecode:: http
  
       DELETE /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/keys/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the key was successfully deleted from the user
   :status 404: the key was not found for the user
   :status 503: one or more subsystems could not be reached








.. http:get:: /users/(uuid:user)/yubikeys

   Lists all install keys for user with given *uuid*.

   **Related permissions**

      users -> UUID -> get

   **Example request**:

    .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/yubikeys HTTP/1.1
     host: cloud.project-fifo.net
     accept: applicaiton/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

    .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

      []

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the user's yubikeys are returned
   :status 404: no yubikeys were found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json array yobikeys: list of yubikeys the user has access to









.. http:put:: /users/(uuid:user)/yubikeys

   Adds a new SSH key to user with given *uuid*.

   **Related permissions**

     users -> UUID -> edit 

.. todo::
    
  Example Requests & Responses still missing.








.. http:delete:: /users/(uuid:user)/yubikeys/(uuid:key)

   Deletes key with given *uuid* for user with given *uuid*.

   **Related permissions**

      users -> UUID -> edit

   **Example request**:

      .. sourcecode:: http
  
       DELETE /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/yubikeys/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the key was successfully deleted from the user
   :status 404: the key was not found for the user
   :status 503: one or more subsystems could not be reached








.. http:get:: /users/(uuid:user)/orgs

   Lists all user orgs.

   *Related permissions**

      users -> ID -> get

   **Example request**:

    .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/orgs HTTP/1.1
     host: cloud.project-fifo.net
     accept: applicaiton/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

    .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

      []

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the user's organizations are returned
   :status 404: no organizations were not found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json array orgs: list of organizations the user is a part of










.. http:put:: /users/<(uuid:user)>/orgs/(uuid:org)

   Joins user with given *uuuid* to org with given *uuid* (optionally sets it to active).

   **Related permissions**

      * users -> ID -> join
      * roles -> ID join

.. todo::
    
  Example Requests & Responses still missing.








.. http:put:: /users/(uuid:user)/metadata[/...]

   Sets a metadata key for user with given *uuid*.

   **Related permissions**

      users -> UUID -> edit

.. todo::
    
  Example Requests & Responses still missing.







.. http:delete:: /users/(uuid:user)/metadata/...

   Removes a key from the metadata for user with given *uuid*.

   **Related permissions**

      users -> UUID -> edit

   **Example request**:

      .. sourcecode:: http
  
       DELETE /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/... HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the metadata key was successfully deleted from the user
   :status 404: the metadata key was not found for the user
   :status 503: one or more subsystems could not be reached



