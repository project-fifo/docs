.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

************
API - DTrace
************

.. http:get:: /dtrace

   Lists all dtrace scripts.

   **Related permissions**

      cloud -> dtrace -> list

.. http:post:: /dtrace

   Creates a dtrace script.

   **Related permissions**

      cloud -> dtrace -> create

.. http:get:: /dtrace/(uuid:dtrace)

   Returns dtrace script for given *uuid*.

   **Related permissions**

      dtrace -> UUID -> get

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
        "name": "zfs reads",
        "script": "/*some dtrace here/*",
        "config": {"start": 0, "end": 64, "step":2},
        "metadata": {}
       }


   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the session information is returned
   :status 403: user is not authoriyed
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached

   :>json string UUID: UUID of DTrace
   :>json string name: name of DTrace
   :>json string script: DTrace scirpt
   :>json object config: DTrace config
   :>json object metadata: metadata associated with DTrace

.. http:put:: /dtrace/(uuid:dtrace)

   Edits dtrace script with given *uuid*.

   **Related permissions**

      dtrace -> UUID -> edit

.. http:delete:: /dtrace/(uuid:dtrace)

   Deletes dtrace script with given *uuid*.

   **Related permissions**

      dtrace -> UUID -> delete

.. http:put:: /dtrace/(uuid:dtrace)/metadata[/...]

   Sets a metadata key for dtrace with given *uuid*.

   **Related permissions**

      dtrace -> UUID -> edit

.. http:delete:: /dtrace/(uuid:dtrace)/metadata/...

   Removes a metadata key for dtrace with given *uuid*.

   **Related permissions**

      dtrace -> UUID -> edit
