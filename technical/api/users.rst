.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

***********
API - Users
***********

.. http:get:: /users

   Lists all users.

   **Related permissions**

   users -> ID -> list 


.. http:post:: /users

   Creates a new user.

   **Related permissions**

   users -> ID -> create


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
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the session information is returned
   :status 404: the session was not found
   :status 403: user is not authoriyed
   :status 503: one or more subsystems could not be reached

   :>json string uuid: UUID of the user that was logged in
   :>json string name: name of the user that was logged in
   :>json array roles: list of role-UUIDs the user is member of
   :>json string org: UUID of the currently active org of the user
   :>json array orgs: list of org-uuid the user is member of
   :>json array permissions: list of permissions this user has
   :>json object keys: SSH public keys registered for this user
   :>json array yubikeys: YubiKey Id's for this user
   :>json object metadata: metadata associated with the user


.. http:put:: /users/(uuid:user)

   Changes password for user with given *uuid*.

   **Related permissions**

   users -> ID -> passwd


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

   :resheader x-snarl-token: the snarl token for this session

   :status 204: the session was successfully deleted
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached

      

.. http:get:: /users/(uuid:user)/permissions

   Lists permissions for user with given *uuid*.

   **Related permissions**

   users -> ID -> get


.. http:put:: /users/(uuid:user)/permissions/<permission>

   Grants <permission> to user with given *uuid*.

   **Related permissions**

   * users -> ID -> grant
   * permissions -> PERMISSIONS -> grant



.. http:delete:: /users/(uuid:user)/permissions/<permission>

   Revokes <permission> for user with given *uuid*.

   **Related permissions**

   * users -> ID -> revoke
   * permissions -> PERMISSION -> revoke


.. http:get:: /users/(uuid:user)/roles

   Lists roles for user with given *uuid*.

   **Related permissions**

   users -> ID -> get


.. http:put:: /users/(uuid:user)/roles/(uuid:role)

   Joins user with given *uuid* to role with given *uuid*.

   **Related permissions**

   * users -> ID -> join
   * roles -> ID -> join


.. http:delete:: /users/(uuid:user)/roles/(uuid:role) 

   Deletes user with given *uuid* from role with given *uuid*.

   **Related permissions**

    * users -> UUID -> edit
    * roles -> ID -> edit


.. http:get:: /users/(uuid:user)/keys

   Lists all install keys for user with given *uuid*.

   **Related permissions**

   users -> UUID -> get


.. http:put:: /users/(uuid:user)/keys

   Adds a new SSH key to user with given *uuid*.

   **Related permissions**

   users -> UUID -> edit


.. http:delete:: /users/(uuid:user)/keys/(uuid:key)

   Deltes key with given *uuid* for user with given *uuid*.

   **Related permissions**

   users -> UUID -> edit


.. http:get:: /users/(uuid:user)/yubikeys

   Lists all install keys for user with given *uuid*.

   **Related permissions**

   users -> UUID -> get


.. http:put:: /users/(uuid:user)/yubikeys

   Adds a new SSH key to user with given *uuid*.

   **Related permissions**

   users -> UUID -> edit 


.. http:delete:: /users/(uuid:user)/yubikeys/(uuid:key)

   Deletes key with given *uuid* for user with given *uuid*.

   **Related permissions**

   users -> UUID -> edit


.. http:get:: /users/(uuid:user)/orgs

   Lists all user orgs.

   *Related permissions**

   users -> ID -> get


.. http:put:: /users/<(uuid:user)>/orgs/(uuid:org)

   Joins user with given *uuuid* to org with given *uuid* (optionally sets it to active).

   **Related permissions**

   * users -> ID -> join
   * roles -> ID join


.. http:put:: /users/(uuid:user)/metadata[/...]

   Sets a metadata key for user with given *uuid*.

   **Related permissions**

   users -> UUID -> edit


.. http:delete:: /users/(uuid:user)/metadata/...

   Removes a key from the metadata for user with given *uuid*.

   **Related permissions**

   users -> UUID -> edit



