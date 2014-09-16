.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
API - IPranges
**************

.. http:get:: /ipranges

   Lists all ipranges.

   **Related permissions**

      cloud -> ipranges -> list

   **Example request**:

   .. sourcecode:: http
  
     GET /ipranges HTTP/1.1
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
   :reqheader x-full-fields: fields to include in the full list - please see: :http:get:`/ipranges/(uuid:iprange)`
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session
   
   :status 200: the IPrange list is returned
   :status 403: user is not authoriyed
   :status 503: one or more subsystems could not be reached

____


.. http:post:: /ipranges

   Creates/Updates an iprange.

   **Related permissions**

      cloud -> ipranges -> create

.. todo::
    
  Example Requests & Responses still missing.

____


.. http:get:: /ipranges/(uuid:iprange)

   Returns iprange details for iprange with given *uuid*.

   **Related permissions**

     ipranges -> NAME -> get

   **Example request**:

   .. sourcecode:: http

     GET /ipranges/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
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
  
      "network": "739faa0d-d098-496c-a87b-dc95520f8d12",
      "netmask": "255.255.255.0",
      "gateway": "192.168.0.1",
      "tag": "admin",
      "vlan": 0,
  
      "free": ["192.168.0.10", "192.168.0.11", "192.168.0.12", "192.168.0.13"],
      "used": ["192.168.0.9", "192.168.0.8"],
      "metadata": {}
     }

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the IPrange information is returned
   :status 403: user is not authoriyed
   :status 404: the IPrange was not found
   :status 503: one or more subsystems could not be reached

   :>json string UUID: UUID of the IPrange
   :>json string name: name of the IPrange
   
   :>json string network: network using the IPrange 
   :>json string netmask: netmask of the network using the IPrange
   :>json string gateway: gateway of the network using the IPrange
   :>json string tag: network tag
   :>json integer vlan: vlan of the network using the IPrange

   :>json array free: list of free IPs
   :>json array used: list of used IPs
   :>json object metadata: metadata associated witht the IPrange

____


.. http:post:: /ipranges/(uuid:iprange)

   Obtains an IP.

   **Related permissions**

      *not needed*

.. todo::
    
  Example Requests & Responses still missing.

____


.. http:delete:: /ipranges/(uuid:iprange)

   Deletes iprange with given *uuid*.

   **Related permissions**

      ipranges -> NAME -> delete

   **Example request**:

   .. sourcecode:: http
  
     DELETE /ipranges/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

   .. sourcecode:: http
  
     HTTP/1.1 204 No Content
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the IPrange was successfully deleted
   :status 404: the IPrange was not found
   :status 503: one or more subsystems could not be reached

____


.. http:delete:: /ipranges/(uuid:iprange)/<ip>

   Releases <IP> from iprange with given *uuid*.

   **Related permissions**

      ipranges -> UUID -> edit

   **Example request**:

   .. sourcecode:: http
  
     DELETE /ipranges/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/<ip> HTTP/1.1
     host: cloud.project-fifo.net
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

   .. sourcecode:: http
  
     HTTP/1.1 204 No Content
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the IP was successfully deleted from the IPrange
   :status 404: the IP was not found
   :status 503: one or more subsystems could not be reached

____


.. http:put:: /ipranges/(uuid:iprange)/metadata[/...]

   Sets a metadata key for iprange with given *uuid*.

   **Related permissions**

      ipranges -> UUID -> edit

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


.. http:delete:: /ipranges/(uuid:iprange)/metadata/...

   Removes a metadata key for iprange with given *uuid*.

   **Related permissions**

      ipranges -> UUID -> edit

   **Example request**:

   .. sourcecode:: http
  
     DELETE /ipranges/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/(path:metadata) HTTP/1.1
     host: cloud.project-fifo.net
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

   .. sourcecode:: http
  
     HTTP/1.1 204 No Content
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the metadata key was successfully deleted from the IPrange
   :status 404: the metadata key was not found
   :status 503: one or more subsystems could not be reached
