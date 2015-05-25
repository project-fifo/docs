.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
API - Packages
**************

.. http:get:: /packages

   Lists all packages.

   **Related permissions**

     cloud -> packages -> list

   **Example request**:

   .. sourcecode:: http

     GET /packages HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
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
   :reqheader x-full-list-fields: fields to include in the full list - please see: :http:get:`/packages/(uuid:package)`
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the package list is returned
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

____


.. http:post:: /packages

   Creates/Updates a package.

   **Related permissions**

      -> packages -> create

   **Example request**:

   .. sourcecode:: http

     POST /api/0.1.0/packages HTTP/1.1
     Accept: application/json
     x-snarl-token: b73b7780-7677-430b-81ef-a57427d166b2
     Content-Type: application/json

     {
     "name":"example",
     "quota":10,
     "ram":1024,
     "cpu_cap":100,
     "requirements":
      [{
      "weight":"must",
      "value":5,
      "attribute":"resources.free-memory",
      "condition":">"
      }],
     "zfs_io_priority":100,
     "block_size":1024,
     "compression":"lz4"}

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 303 See Other
     x-snarl-token: b73b7780-7677-430b-81ef-a57427d166b2
     location: /api/0.1.0/packages/f00ebc46-9026-4a29-aa20-7b4873f5bc8a

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :reqheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the package list is returned
   :status 303: redirect to the session :http:get:`/packages/(uuid:package)`
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json string name: name of the package
   :>json integer quota: size of the zfs dataset
   :>json integer ram: ram available to the package
   :>json integer cpu_cap: CPU Cap *(optional)*
   :>json array requirements: requirements for the package
   :>json string weight:
   :>json integer value:
   :>json string attribute:
   :>json string condition:
   :>json integer zfs_io_priority: ZFS IO priority
   :>json integer blicksize: blocksize of the package
   :>json string compression: compression used for zfs dataset

____


.. http:get:: /packages/(uuid:package)

   Gets details on package with given *uuid*.

   **Related permissions**

     packages -> UUID -> get

   **Example request**:

   .. sourcecode:: http

     GET /packages/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      vary: Accept
      content-type: application/json
      x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

      {
        "uuid": "b7c658e0-2ddb-46dd-8973-4a59ffc9957e",
        "name": "small",

        "blocksizte": 4098,
        "compression": "none",
        "cpu_cap": 100,
        "cpu_shares": 100,
        "max_swap": 1024,
        "quota": 40,
        "ram": 1024,
        "zfs_io_priority": 100,

        "requirements": [],

        "metadata": {}
       }

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the package information is returned
   :status 403: user is not authorized
   :status 404: the package was not found
   :status 503: one or more subsystems could not be reached

   :>json string UUID: UUID of the package
   :>json string name: name of the package

   :>json integer blocksize: blocksize of the package
   :>json string compression: compression used for zfs dataset
   :>json integer cpu_cap: CPU Cap *(optional)*
   :>json integer cpu_shares: CPU Shares *(optional)*
   :>json integer max_swap: max swap
   :>json integer quota: size of the zfs dataset
   :>json integer ram: ram available to the package
   :>json integer zfs_io_priority: ZFS IO priority

   :>json array requirements:

   :>json object metadata: metadata associated with the package

____


.. http:delete:: /packages/(uuid:package)

   Deletes package with given *uuid*.

   **Related permissions**

      packages -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /packages/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the package was successfully deleted
   :status 404: the package was not found
   :status 503: one or more subsystems could not be reached

____


.. http:put:: /packages/(uuid:package)/metadata[/...]

   Sets a metadata key for package with given *uuid*.

   **Related permissions**

     packages -> UUID -> edit

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

   :reqheader accept: the accepted encoding, alias is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :reqheader content-type: the provided datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 204: no content
   :status 404: the VM could not be found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json string <key>: values to store under this key



____


.. http:delete:: /packages/(uuid:package)/metadata/...

   Removes a metadata key for package with given *uuid*.

   **Related permissions**

     packages -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /packages/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/(path:metadata) HTTP/1.1
     host: cloud.project-fifo.net
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the metadata key was successfully deleted from the package
   :status 404: the metadata key was not found

   :status 503: one or more subsystems could not be reached
