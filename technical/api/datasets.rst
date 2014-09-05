.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
API - Datasets
**************

.. http:get:: /datasets

   Lists all datasets.

   **Related permissions**

   cloud -> datasets -> list

.. http:post:: /datasets

   Imports a dataset from dsapi endpoint.

   **Related permissions**

   cloud -> datasets -> import

.. http:get:: /datasets/(uuid:dataset)

   Returns dataset with given *uuid*.

   **Related permissions**

   datasets -> UUID -> get

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
      "type": "",
      "status": "",
      "imported": "",
      "requirements": [],
      "metadata": {},

      "description": "",
      "disk_driver": "",
      "homepage": "",
      "image_size": "",
      "name": "",
      "networks": "",
      "nic_driver": "",
      "os": "",
      "users": "",
      "version": ""
     }

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the session information is returned
   :status 403: user is not authoriyed
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached

.. http:put:: /datasets/(uuid:dataset)

   Cahnges parameters of dataset with given *uuid*.

   **Related permissions**

   datasets -> UUID -> edit

.. http:post:: /datasets/(uuid:dataset)

   Imports a manifest for dataset with given *uuid*.

   **Related permissions**

   datasets -> UUID -> create

.. http:delete:: /datasets/(uuid:dataset)

   Deletes dataset with given *uuid* if not in use.

   **Related permissions**

   datasets -> UUID -> delete

.. http:get:: /datasets/(uuid:dataset)/dataset.gz

   Exports *zvol* for dataset with given *uuid*.

   **Related permissions**

   datasets -> UUID -> export

.. http:put:: /datasets/(uuid:dataset)/dataset.gz

   Imports *zvol* for dataset with given *uuid*.

   **Related permissions**

   datasets -> UUID -> create

.. http:put:: /datasets/(uuid:dataset)/metadata[/...]

   Sets a metadata key for dataset with given *uuid*.

   **Related permissions**

   datasets -> UUID -> edit


.. http:delete:: /datasets/(uuid:dataset)/metadata/...

   Removes a metadata key for dataset with given *uuid*.

   **Related permissions**

   datasets -> UUID -> edit