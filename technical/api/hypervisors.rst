.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*****************
API - Hypervisors
*****************

.. http:get:: /hypervisors

   Returns all hypervisors.

   **Related permissions**

      cloud -> hypervisors -> list

   **Example request**:

   .. sourcecode:: http

     GET /hypervisors HTTP/1.1
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
   :reqheader x-full-list-fields: fields to include in the full list - please see: :http:get:`/hypervisors/(uuid:hypervisor)`
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the organization list is returned
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :<json string uuid: UUID of the hypervisor

____


.. http:get:: /hypervisors/(uuid:hypervisor)

   Returns hypervisor details for hypervisor with given *uuid*.

   **Related permissions**

      hypervisors -> UUID -> get

   **Example request**:

   .. sourcecode:: http

     GET /hypervisors/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json

     {
      "characteristics": {},
      "etherstubs": [],
      "host": "192.168.37.9",
      "metadata": {},
      "alias": "hv1",
      "networks": [],
      "path": [],
      "pools": {},
      "port": 4200,
      "resources": {},
      "services": {},
      "sysinfo": {},
      "uuid": "b7c658e0-2ddb-46dd-8973-4a59ffc9957e",
      "version": "0.6.0",
      "virtualisation": ["zone", "kvm"]
     }

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: the returned datatype, usually ``application/json``

   :status 200: the hypervisor information is returned
   :status 403: user is not authorized
   :status 404: the hypervisor was not found
   :status 503: one or more subsystems could not be reached

   :>json object characteristics: list of hypervisor characteristics
   :>json array etherstubs: list of etherstubs on the hypervisor
   :>json string host: host's IP address
   :>json object metadata: metadata associated with the hypervisor
   :>json string alias: alias of the hypervisor
   :>json array networks: list of networks known to the hypervisor
   :>json array path: path describing the position in the hypervisor graph
   :>json object pools: information about the hosts zpools
   :>json integer port: port number chunter is listening on
   :>json object resources: resources available to the hypervisor
   :>json object services: services and their status on the hypervisor
   :>json object sysinfo: system information about the hypervisor (corresponds to svcs)
   :>json string UUID: UUID of the hypervisor
   :>json string version: Version # of FiFo running on the hypervisor
   :>json array virtualisation: available virtualisation technologies on the hypervisor

____


.. http:delete:: /hypervisors/(uuid:hypervisor)

   Deletes hypervisor with given *uuid*.

   **Related permissions**

     hypervisors -> UUID -> delete

   **Example request**:

   .. sourcecode:: http

     DELETE /hypervisors/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the hypervisor was successfully deleted
   :status 404: the hypervisor was not found
   :status 503: one or more subsystems could not be reached

____


.. http:put:: /hypervisors/(uuid:hypervisor)/config

   Sets hypervisor config for hypervisor with given *uuid*.

   **Related permissions**

     hypervisors -> UUID -> edit

.. todo:: 

 content still missing

____


.. http:put:: /hypervisors/(uuid:hypervisor)/metadata[/...]

   Sets a metadata key for hypervisor with given *uuid*.

   **Related permissions**

      hypervisors -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/metadata/jingles HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6
     Content-Type: application/json

     {
      "notes":  
       [{
        "text":"yap",
        "created_at":"2014-09-13T01:34:03.379Z"
       }]
     }

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     vary: accept

   :reqheader accept: the accepted encoding, alis is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the provided datatype, usually ``application/json``

   :status 204: no content
   :status 404: the hypervisor could not be found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json string <key>: values to store under this key

____


.. http:delete:: /hypervisors/(uuid:hypervisor)/metadata/...

    Removes a key from the metadata for hypervisor with given *uuid*.

   **Related permissions**

      hypervisors -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /hypervisors/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/(path:metadata) HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the metadata key was successfully deleted from the hypervisor
   :status 404: the metadata key was not found
   :status 503: one or more subsystems could not be reached

____


.. http:put:: /hypervisors/(uuid:hypervisor)/characteristics[/...]

   Sets a characteristics key for hypervisor with given *uuid*.

   **Related permissions**

      hypervisors -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     PUT /api/0.1.0/hypervisors/cae242d0-fb7a-4a37-82c7-dcc73ce0fa8d/characteristics HTTP/1.1
     Accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6
     Content-Type: application/json

      {"color": "blue"}

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content
     vary: accept

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :reqheader content-type: the returned datatype, usually ``application/json``

   :status 204: no content
   :status 403: user is not authorized
   :status 404: the hypervisor could not be found.
   :status 503: one or more subsystems could not be reached

   :>json string color: characteristic given to the hypervisor

____


.. http:delete:: /hypervisors/(uuid:hypervisor)/characteristics/...

   Removes a characteristics key for hypervisor with given *uuid*.

   **Related permissions**

      hypervisors -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /hypervisors/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/characteristics/... HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the characteristic was successfully removed from the hypervisor
   :status 404: the characteristic was not found
   :status 503: one or more subsystems could not be reached

____


.. http:delete:: /hypervisors/(uuid:hypervisor)/metadata/...

   Removes a key from the metadata for hypervisor with given *uuid*.

   **Related permissions**

      hypervisors -> UUID -> edit

   **Example request**:

   .. sourcecode:: http

     DELETE /hypervisors/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 204 No Content

   :reqheader authorization: Bearer token for OAuth2 auth

   :status 204: the hypervisor was successfully deleted
   :status 404: the hypervisor was not found
   :status 503: one or more subsystems could not be reached
