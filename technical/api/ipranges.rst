.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
API - IPranges
**************

.. http:get:: /ipranges

   Lists all ipranges.

   **Related permissions**

   cloud -> ipranges -> list

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
      
      "network": "",
      "netmask": "",
      "gateway": "",
      "tag": "",
      "vlan": "",

      "free": [],
      "used": [],
      "metadata": {}
     }

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the session information is returned
   :status 403: user is not authoriyed
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached

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