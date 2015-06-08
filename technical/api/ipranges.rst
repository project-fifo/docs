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
   :reqheader x-full-list-fields: fields to include in the full list - please see: :http:get:`/ipranges/(uuid:iprange)`
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the IPrange list is returned
   :status 403: user is not authorized
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
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the IPrange information is returned
   :status 403: user is not authorized
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
   :>json object metadata: metadata associated with the IPrange

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
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

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
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

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


.. http:delete:: /ipranges/(uuid:iprange)/metadata/...

   Removes a metadata key for iprange with given *uuid*.

   **Related permissions**

      ipranges -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /ipranges/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/(path:metadata) HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the metadata key was successfully deleted from the IPrange
   :status 404: the metadata key was not found
   :status 503: one or more subsystems could not be reached
