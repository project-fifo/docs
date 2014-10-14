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

____

.. http:post:: /roles

   Creates a new role.

   **Related permissions**

      cloud -> roles -> create

   **Example request**:

   .. sourcecode:: http

     POST /api/0.1.0/roles HTTP/1.1
     Accept: application/json
     x-snarl-token: b73b7780-7677-430b-81ef-a57427d166b2
     Content-Type: application/json

     {"name":"Example"}

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 303 See Other
     x-snarl-token: b73b7780-7677-430b-81ef-a57427d166b2
     vary: accept
     location: /api/0.1.0/roles/0d235d16-289a-480e-ab91-2f8c65dabf62

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :reqheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the dataset list is returned
   :status 303: redirect to the session :http:get:`/roles/(uuid:role)`
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json string name: name of the role

____


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

____


.. http:delete:: /roles/(uuid:roles)

   Deletes role with given *uuid*.

   **Related permissions**

     roles -> ID -> delete

   **Example request**:

   .. sourcecode:: http

     DELETE /roles/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the role was successfully deleted
   :status 404: the role was not found
   :status 503: one or more subsystems could not be reached

____


.. http:get:: /roles/(uuid:role)/permissions

   Lists permissions for role with given *uuid*.

   **Related permissions**

      roles -> ID -> get

   **Example request**:

   .. sourcecode:: http

     GET /roles/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/permissions HTTP/1.1
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

   :status 200: the role's permissions are returned
   :status 404: no permissions were found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json array permissions: list of permissions the role is granted

____


.. http:put:: /roles/(uuid:role)/permissions/<permission>

   Grants <permission> for role with given *uuid*.

   **Related permissions**

      * roles -> ID -> grant
      * permissions -> PERMISSION -> grant

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.1.0/roles/0d235d16-289a-480e-ab91-2f8c65dabf62/permissions/groupings/35c4cfbb-057c-455b-93f8-e93205d44ada/edit HTTP/1.1
     Accept: application/json
     x-snarl-token: b73b7780-7677-430b-81ef-a57427d166b2

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 201 Created
     x-snarl-token: b73b7780-7677-430b-81ef-a57427d166b2
     vary: accept

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: no content
   :status 403: user is not authorized
   :status 404: the role could not be found.
   :status 503: one or more subsystems could not be reached

____


.. http:delete:: /roles/(uuid:role)/permissions/<permission>

   Revokes <permission> for role with given *uuid*.

   **Related permissions**

      * users -> ID -> grant
      * permissions -> PERMISSIONS -> revoke

   **Example request**:

   .. sourcecode:: http

     DELETE /roles/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/permissions/roles/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/... HTTP/1.1
     host: cloud.project-fifo.net

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the permission was successfully revoked from the role
   :status 404: the permission was not found for that role
   :status 503: one or more subsystems could not be reached

____


.. http:put:: /roles/(uuid:role)/metadata[/...]

   Sets a metadata key for role with given *uuid*.

   **Related permissions**

      roles -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/metadata/jingles HTTP/1.1
     Accept: application/json
     x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
     Content-Type: application/json

     {"notes":  [{"text":"yap","created_at":"2014-09-13T01:34:03.379Z"}]}

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
     vary: accept

   :reqheader accept: the accepted encoding, alis is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :reqheader content-type: the provided datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 204: no content
   :status 404: the VM could not be found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json string <key>: values to store under this key

____


.. http:delete:: /roles/(uuid:role)/metadata/...

   Removes a key from the metadata for role with given *uuid*.

   **Related permissions**

      roles -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /roles/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/... HTTP/1.1
     x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
     host: cloud.project-fifo.net

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the metadata key was successfully deleted from that role
   :status 404: the metadata key was not found for that role
   :status 503: one or more subsystems could not be reached
