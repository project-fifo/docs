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
   :reqheader x-full-list-fields: fields to include in the full list - please see: :http:get:`/users/(uuid:user)`
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the user list is returned
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

____


.. http:post:: /users

   Creates a new user.

   **Related permissions**

      users -> ID -> create

   **Example request**:

   .. sourcecode:: http

     POST /api/0.2.0/users HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6
     Content-Type: application/json

     {
     "user":"Example",
     "password":"secret"
     }

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 303 See Other
     vary: accept
     location: /api/0.2.0/users/7df734a1-7332-45da-aa2a-9a3d856fa58a

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the returned datatype, usually ``application/json``

   :status 200: the user list is returned
   :status 303: redirect to the session :http:get:`/users/(uuid:user)`
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json string user: name of the new user
   :>json string password: password for the new user

____


.. http:get:: /users/(uuid:user)

   Returns user with given *uuid*.

   **Related permissions**

      users -> ID -> get

   **Example request**:

   .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json

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
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the user information is returned
   :status 404: the user was not found
   :status 403: user is not authorized
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

____


.. http:put:: /users/(uuid:user)

   Changes password for user with given *uuid*.

   **Related permissions**

      users -> ID -> passwd

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.2.0/users/7df734a1-7332-45da-aa2a-9a3d856fa58a HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6
     Content-Type: application/json

     {"password":"top secret"}

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the returned datatype, usually ``application/json``

   :status 204: no content
   :status 403: user is not authorized
   :status 404: the user could not be found.
   :status 503: one or more subsystems could not be reached

   :>json string password: password you want to set for the user

____


.. http:delete:: /users/(uuid:user)

   Deletes user with given *uuid*.

   **Related permissions**

    users -> ID -> delete

   **Example request**:

   .. sourcecode:: http

     DELETE /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the user was successfully deleted
   :status 404: the user was not found
   :status 503: one or more subsystems could not be reached

____


.. http:get:: /users/(uuid:user)/permissions

   Lists permissions for user with given *uuid*.

   **Related permissions**

     users -> ID -> get

   **Example request**:

   .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/permissions HTTP/1.1
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

   :status 200: the user information is returned
   :status 404: the user was not found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :<json array permissions: list of permissions the user is granted

____


.. http:get:: /users/(uuid:user)/permissions/...

   Tests if a user has is allowed to perform an action. Please not that 403 here does
   **not** mean the user is not allowed but that the requestig token was not allowed to
   test for the user!

   Please be aware that of the two keys returned (`ok` and `error`) only one is ever present at a time.

   **Related permissions**

     users -> ID -> get

   **Example request**:

   .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/permissions/cloud/vms/create HTTP/1.1
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

   :status 200: the user information is returned
   :status 404: the user was not found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :<json string ok: set to ``"allowed"`` when `user` has the permission in question
   :<json string error: set to ``"forbidden"`` when `user` **does not have** the permission in question

____


.. http:put:: /users/(uuid:user)/permissions/<permission>

   Grants <permission> to user with given *uuid*.

   **Related permissions**

     * users -> ID -> grant
     * permissions -> PERMISSIONS -> grant

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.2.0/users/7df734a1-7332-45da-aa2a-9a3d856fa58a/permissions/groupings/35c4cfbb-057c-455b-93f8-e93205d44ada/get HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 201 Created
     vary: accept

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth

   :status 200: the check was performed
   :status 403: user requesting the check is not authorized
   :status 404: the checked user could not be found.
   :status 503: one or more subsystems could not be reached


____


.. http:delete:: /users/(uuid:user)/permissions/<permission>

   Revokes <permission> for user with given *uuid*.

   **Related permissions**

      * users -> ID -> revoke
      * permissions -> PERMISSION -> revoke

   **Example request**:

   .. sourcecode:: http

     DELETE /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/permissions/users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/... HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the permission was successfully revoked from the user
   :status 404: the permission was not found for that user
   :status 503: one or more subsystems could not be reached

____


.. http:get:: /users/(uuid:user)/roles

   Lists roles for user with given *uuid*.

   **Related permissions**

      users -> ID -> get

   **Example request**:

   .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/roles HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json

      [91740c18-3a33-11e4-b9cf-13aca1131ce1]

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: user's roles are returned
   :status 404: no roles were found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json array roles: list of roles the user is part of

____


.. http:put:: /users/(uuid:user)/roles/(uuid:role)

   Joins user with given *uuid* to role with given *uuid*.

   **Related permissions**

     * users -> ID -> join
     * roles -> ID -> join

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.2.0/users/7df734a1-7332-45da-aa2a-9a3d856fa58a/roles/094a757b-84cd-46df-92bb-279a943fa489 HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     vary: accept

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: no content
   :status 403: user is not authorized
   :status 404: the user could not be found.
   :status 503: one or more subsystems could not be reached

____


.. http:delete:: /users/(uuid:user)/roles/(uuid:role)

   Deletes role with given *uuid* from user with given *uuid*.

   **Related permissions**

      * users -> UUID -> edit
      * roles -> ID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/roles/c7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the role was successfully deleted for the user
   :status 404: the role was not found for the user
   :status 503: one or more subsystems could not be reached

____


.. http:get:: /users/(uuid:user)/keys

   Lists all install keys for user with given *uuid*.

   **Related permissions**

      users -> UUID -> get

   **Example request**:

   .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/keys HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json

      {"key-id": "ssh-rsa ..."}

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the user's keys are returned
   :status 404: the user was not found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json object keys: list of keys the user has access to

____


.. http:put:: /users/(uuid:user)/keys

   Adds a new SSH key to user with given *uuid*.

   **Related permissions**

      users -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.2.0/users/7df734a1-7332-45da-aa2a-9a3d856fa58a/keys HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6
     Content-Type: application/json

     {
     "test@testhost":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDZyw2HsD2TBPpBcCJLge4Eu1N9IXHx0S9APSdC4GEre3h4huNT9LUA78oOB1LDIyqmwbHy5yqVVBht4awmcveaSsBIDEPBrU+ZrSeibg3ikQxBYA+7IG8gwvEqxI9EdbnF6eqstfiUIaLsLuUY2E2b2DGIohy/NIw0tccchLR0kHUGz4yjmMZg78X9ux2VqFhlTfj3xDsagxFjo90FQkrO32SLULFS9fG5Ki8vsvhfkhhtgct74i894lj4DRThqmvgygODXcyvi/wtixaqKqcn+Y1JCr5AsvXvYmWQzdRh9Rv77j0mleo0xqosqXIH1HqsM4CJmdYGCPU7JB6k0j/H test@testhost"
     }

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     Content-Type: application/json
     vary: accept

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the returned datatype, usually ``application/json``

   :status 204: no content
   :status 400: the ssh key was badly formated
   :status 403: user is not authorized
   :status 404: the user could not be found.
   :status 503: one or more subsystems could not be reached

   :>json string key: key that is to be added to the user, it needs to have the form 'ssh-rsa <key data> <id>'

____


.. http:delete:: /users/(uuid:user)/keys/(uuid:key)

   Deltes key with given *uuid* for user with given *uuid*.

   **Related permissions**

      users -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/keys/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the key was successfully deleted from the user
   :status 404: the key was not found for the user
   :status 503: one or more subsystems could not be reached

____


.. http:get:: /users/(uuid:user)/yubikeys

   Lists all install keys for user with given *uuid*.

   **Related permissions**

      users -> UUID -> get

   **Example request**:

   .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/yubikeys HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json

      []

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the user's yubikeys are returned
   :status 404: no yubikeys were found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json array yobikeys: list of yubikeys the user has access to

____


.. http:put:: /users/(uuid:user)/yubikeys

   Adds a new SSH key to user with given *uuid*.

   **Related permissions**

   **Exmaple request**:

   .. sourcecode:: http

     PUT /api/0.2.0/users/7df734a1-7332-45da-aa2a-9a3d856fa58a/yubikeys HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6
     Content-Type: application/json

     {"otp":"someyubikeyotp"}

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the returned datatype, usually ``application/json``

   :status 204: no content
   :status 403: user is not authorized
   :status 404: the user could not be found.
   :status 503: one or more subsystems could not be reached

   :>json string key: YUBIkey OTP that is to be added to the user


____


.. http:delete:: /users/(uuid:user)/yubikeys/(uuid:key)

   Deletes key with given *uuid* for user with given *uuid*.

   **Related permissions**

      users -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

      DELETE /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/yubikeys/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
      host: cloud.project-fifo.net
      Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the key was successfully deleted from the user
   :status 404: the key was not found for the user
   :status 503: one or more subsystems could not be reached

____


.. http:get:: /users/(uuid:user)/orgs

   Lists all user orgs.

   *Related permissions**

      users -> ID -> get

   **Example request**:

   .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/orgs HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json

      [916a16ae-3a33-11e4-9d26-1ffeeda8b532]

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the user's organizations are returned
   :status 404: no organizations were not found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json array orgs: list of organizations the user is a part of

____


.. http:put:: /users/<(uuid:user)>/orgs/(uuid:org)

   Joins user with given *uuuid* to org with given *uuid* (optionally sets it to active).

   **Related permissions**

      * users -> ID -> join
      * roles -> ID join

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.2.0/users/7df734a1-7332-45da-aa2a-9a3d856fa58a/orgs/c1590249-1b2c-4440-952b-eef2ff83ed1f HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     vary: accept

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: no content
   :status 403: user is not authorized
   :status 404: the user could not be found.
   :status 503: one or more subsystems could not be reached

____


.. http:put:: /users/(uuid:user)/metadata[/...]

   Sets a metadata key for user with given *uuid*.

   **Related permissions**

      users -> UUID -> edit

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
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json string <key>: values to store under this key

____


.. http:delete:: /users/(uuid:user)/metadata/...

   Removes a key from the metadata for user with given *uuid*.

   **Related permissions**

      users -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/... HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the metadata key was successfully deleted from the user
   :status 404: the metadata key was not found for the user
   :status 503: one or more subsystems could not be reached
