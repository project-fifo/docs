.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
API - Networks
**************

.. http:get:: /networks

   Lists all networks.

   **Related permissions**

     cloud -> networks -> list

   **Example request**:

      .. sourcecode:: http
  
        GET /networks HTTP/1.1
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
   :reqheader x-full-fields: fields to include in the full list - please see: :http:get:`/networks/(uuid:network)`
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session
   
   :status 200: the user list is returned
   :status 403: user is not authoriyed
   :status 503: one or more subsystems could not be reached

____







.. http:post:: /networks

   Create a new network.

   **Related permissions**

      cloud -> networks -> create

.. todo::
    
  Example Requests & Responses still missing.

____







.. http:get:: /networks/(uuid:network)

   Returns network details for network with given *uuid*.

   **Related permissions**

      networks -> UUID -> get

   **Example request**:

    .. sourcecode:: http

     GET /networks/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
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

   :status 200: the network's information is returned
   :status 403: user is not authoriyed
   :status 404: the network was not found
   :status 503: one or more subsystems could not be reached


   :>json string UUID: UUID of the network
   :>json string name: name of the network
   :>json array ipranges: IP ranges for the network
   :>json object metadata: metadata associated with the network

____








.. http:delete:: /networks/(uuid:network)

   Deletes network with given *uuid*.

   **Related permissions**

      networks -> UUID -> delete

   **Example request**:

      .. sourcecode:: http
  
       DELETE /networks/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the network was successfully deleted
   :status 404: the network was not found
   :status 503: one or more subsystems could not be reached

____
   







.. http:put:: /networks/(uuid:network)/ipranges/<iprange>

   Adds an <iprange> for network with given *uuid*.

   **Related permissions**

      networks -> UUID -> edit

.. todo::
    
  Example Requests & Responses still missing.

____








.. http:delete:: /networks/(uuid:network)/ipranges/<iprange>

   Removes an <iprange> from for network with given *uuid*.

   **Related permissions**

      networks -> UUID -> edit

   **Example request**:

      .. sourcecode:: http
  
       DELETE /networks/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/ipranges/<iprange> HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the IPrange was successfully removed from the network
   :status 404: the IPrange was not found
   :status 503: one or more subsystems could not be reached

____







.. http:put:: /networks/(uuid:network)/metadata[/...]

   Sets a metadata key for network with given *uuid*.

   **Related permissions**

      networks -> UUID -> edit

.. todo::
    
  Example Requests & Responses still missing.

____







.. http:delete:: /networks/(uuid:network)/metadata/...

   Removes a metadata key for network with given *uuid*.

   **Related permissions**

      networks -> UUID -> edit

   **Example request**:

      .. sourcecode:: http
  
       DELETE /networks/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/(path:metadata) HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the metadata key was successfully deleted from the network
   :status 404: the metadata key  was not found
   :status 503: one or more subsystems could not be reached

____
