.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
API - Networks
**************

.. http:get:: /networks

   Lists all networks.

   **Related permissions**

     cloud -> networks -> list

.. http:post:: /networks

   Create a new network.

   **Related permissions**

      cloud -> networks -> create  


.. http:get:: /networks/(uuid:network)

   Returns network details for network with given *uuid*.

   **Related permissions**

      networks -> UUID -> get

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
      "name": "Intranet",
      "iprages": ["daf72785-000b-4abb-8f30-d862405d3bb2", "e1618837-be96-4e10-8c5f-41c223607c65"],
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


   :>json string UUID: UUID of the network
   :>json string name: name of the network
   :>json array ipranges: IP ranges for the network
   :>json object metadata: metadata associated with the network


.. http:delete:: /networks/(uuid:network)

   Deletes network with given *uuid*.

   **Related permissions**

      networks -> UUID -> delete


.. http:put:: /networks/(uuid:network)/ipranges/<iprange>

   Adds an <iprange> for network with given *uuid*.

   **Related permissions**

      networks -> UUID -> edit


.. http:delete:: /networks/(uuid:network)/ipranges/<iprange>

   Removes an <iprange> from for network with given *uuid*.

   **Related permissions**

      networks -> UUID -> edit

.. http:put:: /networks/(uuid:network)/metadata[/...]

   Sets a metadata key for network with given *uuid*.

   **Related permissions**

      networks -> UUID -> edit


.. http:delete:: /networks/(uuid:network)/metadata/...

   Removes a metadata key for network with given *uuid*.

   **Related permissions**

      networks -> UUID -> edit
