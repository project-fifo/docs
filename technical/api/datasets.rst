.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
API - Datasets
**************

.. http:get:: /datasets

   Lists all datasets.

   **Related permissions**

      cloud -> datasets -> list

   **Example request**:

   .. sourcecode:: http

     GET /datasets HTTP/1.1
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
   :reqheader x-full-list-fields: fields to include in the full list - please see: :http:get:`/datasets/(uuid:dataset)`
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the dataset list is returned
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

____


.. http:post:: /datasets

   Imports a dataset from dsapi endpoint.

   **Related permissions**

    cloud -> datasets -> import

   **Example request**:

   .. sourcecode:: http

      POST /api/0.1.0/datasets HTTP/1.1
      accept: application/json
      content-type: application/json
      Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

      {"url": "https://datasets.at/datasets/21274016-2ad3-11e4-9673-e3abad521cc2"}

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 303 See Other
      content-type: application/json
      location: /api/0.1.0/datasets/21274016-2ad3-11e4-9673-e3abad521cc2

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the returned datatype, usually ``application/json``

   :status 200: the dataset list is returned
   :status 303: redirect to the session :http:get:`/datasets/(uuid:dataset)`
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

____


.. http:get:: /datasets/(uuid:dataset)

   Returns dataset with given *uuid*.

   **Related permissions**

      datasets -> UUID -> get

   **Example request**:

   .. sourcecode:: http

     GET /datasets/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
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
      "type": "kvm",
      "status": "imported",
      "imported": 1,
      "requirements": [],
      "metadata": {},
      "description": "",
      "disk_driver": "virtio",
      "homepage": "",
      "image_size": 12345,
      "name": "example",
      "networks": {"net0":"public"},
      "nic_driver": "virtio",
      "os": "linux",
      "users": ["root", "admin"],
      "version": "0.1.0"
     }

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the dataset information is returned
   :status 403: user is not authorized
   :status 404: the dataset was not found
   :status 503: one or more subsystems could not be reached

   :>json string UUID: UUID of the dataset
   :>json string type: type of the dataset
   :>json string status: import status of the dataset (pending / importing / imported / failed)
   :>json integer imported: percentage of dataset imported (0 .. 1)
   :>json array requirements: requirements for the dataset
   :>json object metadata: metadate associated with the dataset
   :>json string description: description of the dater set
   :>json string disk_driver: disk driver to use for kvms
   :>json string homepage: homepage of the dataset
   :>json integer image_size: size of the image
   :>json string name: name of the dataset
   :>json object networks: networks/interfaces the dataset requires
   :>json string nic_driver: nic driver to use for kvms
   :>json string os: dataset OS
   :>json array users: users provided by the dataset
   :>json string version: version # of the dataset

____

.. http:put:: /datasets/(uuid:dataset)

   Cahnges parameters of dataset with given *uuid*.

   **Related permissions**

      datasets -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

      PUT /api/0.1.0/datasets/21274016-2ad3-11e4-9673-e3abad521cc2 HTTP/1.1
      Accept: application/json
      Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6
      Content-Type: application/json

      {
      "networks": 
       [{
       "description":"public", 
       "name":"net0"}, 
       {"name":"net1",
       "description":"internal"
       }]
      }

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 204 No Content
      vary: accept

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the returned datatype, usually ``application/json``

   :status 204: no content
   :status 403: user is not authorized
   :status 404: the dataset could not be found.
   :status 503: one or more subsystems could not be reached

   :>json string networks: contains information about the network
   :>json string description: contains a description of the network
   :>json string name: name of the network

____


.. http:post:: /datasets/(uuid:dataset)

   Imports a manifest for dataset with given *uuid*.

   **Related permissions**

      datasets -> UUID -> create

   **Example request**:

   .. note::

    Input is a DS manifest.

   .. sourcecode:: http

      POST /api/0.1.0/datasets/d34c301e-10c3-11e4-9b79-5f67ca448df0 HTTP/1.1
      accept: application/json
      content-type: application/json
      Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

      {
      "uuid":"d34c301e-10c3-11e4-9b79-5f67ca448df0",
      "name":"base64",
      "version":"14.2.0",
      "description":"A 64-bit SmartOS image with just essential packages installed. Ideal for users who are comfortable with setting up their own environment and tools.",
      "os":"smartos",
      "type":"zone-dataset",
      "homepage":"http://wiki.joyent.com/jpc2/SmartMachine+Base",
      "urn":"sdc:sdc:base64:14.2.0",
      "published_at":"2014-07-21T10:43:17Z",
      "created_at":"2014-07-21T10:43:17Z",
      "creator_uuid":"00000000-0000-0000-0000-000000000000",
      "creator_name":"sdc",
      "vendor_uuid":"00000000-0000-0000-0000-000000000000",
      "requirements":
       {
       "networks":
        [{
        "description":"public",
        "name":"net0"
        }]
       },
       "files":
        [{
        "url":"http://datasets.at/datasets/d34c301e-10c3-11e4-9b79-5f67ca448df0/base64-14.2.0.zfs.gz",
        "path":"base64-14.2.0.zfs.gz",
        "md5":"a514917b3e6b8e18f8b21648a19876dc",
        "sha1":"97b2eec4bf8e9ae8c4be43e32c8672be198278d6","size":116062401
        }]
      }

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 303 See Other
      location: /api/0.1.0/datasets/21274016-2ad3-11e4-9673-e3abad521cc2

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the returned datatype, usually ``application/json``

   :status 200: the dataset list is returned
   :status 303: redirect to the session :http:get:`/datasets/(uuid:dataset)`
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached


____


.. http:delete:: /datasets/(uuid:dataset)

   Deletes dataset with given *uuid* if not in use.

   **Related permissions**

      datasets -> UUID -> delete

   **Example request**:

   .. sourcecode:: http

     DELETE /datasets/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the dataset was successfully deleted
   :status 404: the dataset was not found
   :status 503: one or more subsystems could not be reached

____


.. http:get:: /datasets/(uuid:dataset)/dataset.gz

   Exports *zvol* for dataset with given *uuid*.

   **Related permissions**

      datasets -> UUID -> export

   **Example request**:

   .. sourcecode:: http

     GET /datasets/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/dataset.gz HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/x-gzip
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/x-gzip

      ... binary data ...

   :reqheader accept: the accepted encoding, valid is ``application/x-gzip``
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/x-gzip``

   :status 200: the organization's triggers are returned
   :status 404: the triggers were not found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

____


.. http:put:: /datasets/(uuid:dataset)/dataset.gz

   Imports *zvol* for dataset with given *uuid*.

   **Related permissions**

      datasets -> UUID -> create

.. todo::

  Example Requests & Responses still missing.

____


.. http:put:: /datasets/(uuid:dataset)/metadata[/...]

   Sets a metadata key for dataset with given *uuid*.

   **Related permissions**

      datasets -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/metadata/jingles HTTP/1.1
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


.. http:delete:: /datasets/(uuid:dataset)/metadata/...

   Removes a metadata key for dataset with given *uuid*.

   **Related permissions**

      datasets -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /datasets/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/(path:metadata) HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the metadata key was successfully deleted from the dataset
   :status 404: the metadata key was not found
   :status 503: one or more subsystems could not be reached
