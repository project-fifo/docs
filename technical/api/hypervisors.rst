.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*****************
API - Hypervisors
*****************

.. http:get:: /hypervisors

   Returns all hypervisors.

   **Related permissions**

   cloud -> hypervisors -> list


.. http:get:: /hypervisors/(uuid:hypervisor)

   Returns hypervisor details for hypervisor with given *uuid*.

   **Related permissions**

   hypervisors -> UUID -> get

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
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the session information is returned
   :status 403: user is not authoriyed
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached

   :>json string uuid: UUID of the hypervisor
   :>json object characteristics: lists the characteristics of that hypervisor
   :>json string host: ???
   :>json object metadata: metadata associated with the user
   :>json string alias: ???
   :>json array networks:
   :>json array path:
   :>json object pools:
   :>json string port:
   :>json object resources:
   :>json array services:
   :>json array sysinfo:
   :>json string version:
   :>json array virtualization:

.. http:put:: /hypervisors/(uuid:hypervisor)/config

   Sets hypervisor config for hypervisor with given *uuid*.

   **Related permissions**

   hypervisors -> UUID -> edit


.. http:put:: /hypervisors/(uuid:hypervisor)/metadata[/...]

   Sets a metadata key for hypervisor with given *uuid*.

   **Related permissions**

   hypervisors -> UUID -> edit


.. http:delete:: /hypervisors/(uuid:hypervisor)/metadata/...

   Removes a key from the metadata for hypervisor with given *uuid*.

   **Related permissions**

   hypervisors -> UUID -> edit

.. note::

   Characteristics are used to describe capabilities of the hypervisor for the selection process.

.. http:put:: /hypervisors/(uuid:hypervisor)/characteristics[/...]

   Sets a characteristics key for hypervisor with given *uuid*.

   **Related permissions**


   hypervisors -> UUID -> edit

.. http:delete:: /hypervisors/(uuid:hypervisor)/characteristics/...

   Removes a characteristics key for hypervisor with given *uuid*.

   **Related permissions**

   hypervisors -> UUID -> edit


.. http:delete:: /hypervisors/(uuid:hypervisor)/metadata/...

   Removes a key from the metadata for hypervisor with given *uuid*.

   **Related permissions**

   hypervisors -> UUID -> edit
