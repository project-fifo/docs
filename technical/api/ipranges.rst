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

.. http:post:: /ipranges

   Creates/Updates an iprange.

   **Related permissions**

      cloud -> ipranges -> create


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


.. http:post:: /ipranges/(uuid:iprange)

   Obtains an IP.

   **Related permissions**


.. http:delete:: /ipranges/(uuid:iprange)

   Deletes iprange with given *uuid*.

   **Related permissions**

      ipranges -> NAME -> delete


.. http:delete:: /ipranges/(uuid:iprange)/<ip>

   Releases <IP> fron iprange with given *uuid*.

   **Related permissions**

      ipranges -> UUID -> edit


.. http:put:: /ipranges/(uuid:iprange)/metadata[/...]

   Sets a metadata key for iprange with given *uuid*.

   **Related permissions**

      ipranges -> UUID -> edit


.. http:delete:: /ipranges/(uuid:iprange)/metadata/...

   Removes a metadata key for iprange with given *uuid*.

   **Related permissions**

      ipranges -> UUID -> edit