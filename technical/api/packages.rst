.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
API - Packages
**************

.. http:get:: /packages

   Lists all packages.

   **Related permissions**

   cloud -> packages -> list

.. http:post:: /packages

   Creates/Updates a package.

   **Related permissions**

   cloud -> packages -> create

.. http:get:: /packages/(uuid:package)

   Gets details on package with given *uuid*.

   **Related permissions**

   packages -> UUID -> get

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

      "blocksizte": "",
      "compression": "",
      "cup_cap": "",
      "cpu_shares": "",
      "max_swap": "",
      "quota": "",
      "ram": "",
      "zfs_io_priorety": "",

      "requirements": [],

      "metadata": {}
     }

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the session information is returned
   :status 403: user is not authoriyed
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached

.. http:delete:: /packages/(uuid:package)

   Deletes package with given *uuid*.

   **Related permissions**

   packages -> UUID -> edit


.. http:put:: /packages/(uuid:package)/metadata[/...]

   Sets a metadata key for package with given *uuid*.

   **Related permissions**

   packages -> UUID -> edit


.. http:delete:: /packages/(uuid:package)/metadata/...

   Removes a metadata key for package with given *uuid*.

   **Related permissions**

   packages -> UUID -> edit