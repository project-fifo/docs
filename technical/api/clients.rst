.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*************
API - Clients
*************

.. http:get:: /clients

   Lists all clients.

   **Related permissions**

      cloud -> clients -> list

   **Example request**:

   .. sourcecode:: http

     GET /clients HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json

     ["b7c658e0-2ddb-46dd-8973-4a59ffc9957e"]


   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader x-full-list: true - to get a full list instead of UUIDs
   :reqheader x-full-list-fields: fields to include in the full list - please see: :http:get:`/clients/(uuid:client)`
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the client list is returned
   :status 403: client is not authorized
   :status 503: one or more subsystems could not be reached

____

.. http:post:: /clients

   Creates a new client.

   **Related permissions**

      cloud -> clients -> create

   **Example request**:

   .. sourcecode:: http

     POST /api/0.2.0/clients HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6
     Content-Type: application/json

     {
     "client":"Example",
     "secret":"secret"
     }

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 303 See Other
     vary: accept
     location: /api/0.2.0/clients/7df734a1-7332-45da-aa2a-9a3d856fa58a

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the returned datatype, usually ``application/json``

   :status 200: the client list is returned
   :status 303: redirect to the session :http:get:`/clients/(uuid:client)`
   :status 403: client is not authorized
   :status 503: one or more subsystems could not be reached

   :>json string client: name of the new client
   :>json string password: password for the new client

____

.. http:get:: /clients/(uuid:client)

   Returns client with given *uuid*.

   **Related permissions**

      clients -> ID -> get

   **Example request**:

   .. sourcecode:: http

     GET /clients/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json

     {"client_id": "Client1",
     "metadata": {},
     "name": "Client1",
     "permissions": [],
     "redirect_uris" ["http://test.com"]
     "roles": [],
     "type": "public",
     "uuid": "af7d83bd-93d4-4d16-998f-7fbd3cc756cf"
     }

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the client information is returned
   :status 404: the client was not found
   :status 403: client is not authorized
   :status 503: one or more subsystems could not be reached

   :>json string uuid: UUID of the client that is logged in
   :>json string name: name of the client that is logged in
   :>json array roles: list of role-UUIDs the client is a member of
   :>json string org: UUID of the currently active organization of the client
   :>json array orgs: list of org-uuid the client is member of
   :>json array permissions: list of permissions the client is granted
   :>json object keys: SSH public keys registered for the client
   :>json array yubikeys: YubiKey ID's for the client
   :>json object metadata: metadata associated with the client

____


.. http:put:: /clients/(uuid:client)

   Changes secret for client with given *uuid*.

   **Related permissions**

      clients -> ID -> passwd

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.2.0/clients/7df734a1-7332-45da-aa2a-9a3d856fa58a HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6
     Content-Type: application/json

     {"secret":"top secret"}

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the returned datatype, usually ``application/json``

   :status 204: no content
   :status 403: client is not authorized
   :status 404: the client could not be found.
   :status 503: one or more subsystems could not be reached

   :>json string password: password you want to set for the client

____


.. http:delete:: /clients/(uuid:client)

   Deletes client with given *uuid*.

   **Related permissions**

    clients -> ID -> delete

   **Example request**:

   .. sourcecode:: http

     DELETE /clients/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the client was successfully deleted
   :status 404: the client was not found
   :status 503: one or more subsystems could not be reached

____


.. http:get:: /clients/(uuid:client)/permissions

   Lists permissions for client with given *uuid*.

   **Related permissions**

     clients -> ID -> get

   **Example request**:

   .. sourcecode:: http

     GET /clients/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/permissions HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json

      [["..."]]


   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the client information is returned
   :status 404: the client was not found
   :status 403: client is not authorized
   :status 503: one or more subsystems could not be reached

   :<json array permissions: list of permissions the client is granted

____


.. http:get:: /clients/(uuid:client)/permissions/...

   Tests if a client has is allowed to perform an action. Please not that 403 here does
   **not** mean the client is not allowed but that the requestig token was not allowed to
   test for the client!

   Please be aware that of the two keys returned (`ok` and `error`) only one is ever present at a time.

   **Related permissions**

     clients -> ID -> get

   **Example request**:

   .. sourcecode:: http

     GET /clients/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/permissions/cloud/vms/create HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json

      {ok: "allowed", error: "forbidden"}


   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the client information is returned
   :status 404: the client was not found
   :status 403: client is not authorized
   :status 503: one or more subsystems could not be reached

   :<json string ok: set to ``"allowed"`` when `client` has the permission in question
   :<json string error: set to ``"forbidden"`` when `client` **does not have** the permission in question

____


.. http:put:: /clients/(uuid:client)/permissions/<permission>

   Grants <permission> to client with given *uuid*.

   **Related permissions**

     * clients -> ID -> grant
     * permissions -> PERMISSIONS -> grant

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.2.0/clients/7df734a1-7332-45da-aa2a-9a3d856fa58a/permissions/groupings/35c4cfbb-057c-455b-93f8-e93205d44ada/get HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 201 Created
     vary: accept

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth

   :status 200: the check was performed
   :status 403: client requesting the check is not authorized
   :status 404: the checked client could not be found.
   :status 503: one or more subsystems could not be reached


____


.. http:delete:: /clients/(uuid:client)/permissions/<permission>

   Revokes <permission> for client with given *uuid*.

   **Related permissions**

      * clients -> ID -> revoke
      * permissions -> PERMISSION -> revoke

   **Example request**:

   .. sourcecode:: http

     DELETE /clients/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/permissions/clients/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/... HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the permission was successfully revoked from the client
   :status 404: the permission was not found for that client
   :status 503: one or more subsystems could not be reached

____


.. http:POST:: /clients/<(uuid:client)>/uris/(uuid:org)

   Adds an URI to the valid redirect URI's for this cleint

   **Related permissions**

      * clients -> ID -> edit

   **Example request**:

   .. sourcecode:: http

     POST /api/0.2.0/clients/7df734a1-7332-45da-aa2a-9a3d856fa58a/uris/127657  HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

     {"uri": "https://project-fifo.net"}

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     vary: accept

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: no content
   :status 403: client is not authorized
   :status 404: the client could not be found.
   :status 503: one or more subsystems could not be reached

____


.. http:delete:: /clients/(uuid:client)/uris/(integer:uri-key)

   Deletes key with given *uuid* for client with given *uuid*.

   **Related permissions**

      clients -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

      DELETE /clients/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/uris/12378576 HTTP/1.1
      host: cloud.project-fifo.net
      Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the key was successfully deleted from the client
   :status 404: the key was not found for the client
   :status 503: one or more subsystems could not be reached

____


.. http:put:: /clients/(uuid:client)/metadata[/...]

   Sets a metadata key for client with given *uuid*.

   **Related permissions**

      clients -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.2.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/metadata/jingles HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6
     Content-Type: application/json

     {"notes":  [{"text":"yap","created_at":"2014-09-13T01:34:03.379Z"}]}

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     vary: accept

   :reqheader accept: the accepted encoding, alias is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the provided datatype, usually ``application/json``

   :status 204: no content
   :status 404: the VM could not be found
   :status 403: client is not authorized
   :status 503: one or more subsystems could not be reached

   :>json string <key>: values to store under this key

____


.. http:delete:: /clients/(uuid:client)/metadata/...

   Removes a key from the metadata for client with given *uuid*.

   **Related permissions**

      clients -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /clients/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/... HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the metadata key was successfully deleted from the client
   :status 404: the metadata key was not found for the client
   :status 503: one or more subsystems could not be reached
