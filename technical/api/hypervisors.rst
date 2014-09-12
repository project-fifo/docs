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
   :reqheader x-full-fields: fields to include in the full list - please see: :http:get:`/hypervisors/(uuid:hypervisor)`
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session
   
   :status 200: the organization list is returned
   :status 403: user is not authoriyed
   :status 503: one or more subsystems could not be reached







.. http:get:: /hypervisors/(uuid:hypervisor)

   Returns hypervisor details for hypervisor with given *uuid*.

   **Related permissions**

      hypervisors -> UUID -> get

   **Example request**:

      .. sourcecode:: http

       GET /hypervisors/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
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
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the hypervisoer information is returned
   :status 403: user is not authoriyed
   :status 404: the hypervisor was not found
   :status 503: one or more subsystems could not be reached

   :>json object characteristics: list of hypervisor characteristics
   :>json array etherstubs: list of etherstubs on the hypervisor
   :>json string host: host's IP adress
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








.. http:delete:: /hypervisors/(uuid:hypervisor)

    Deletes hypervisor with given *uuid*.

    **Related permissions**

    hypervisors -> UUID -> delete

    **Example request**:

      .. sourcecode:: http
  
        DELETE /hypervisors/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
        host: cloud.project-fifo.net

    **Example response**:

      .. sourcecode:: http
  
        HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the hypervisor was successfully deleted
   :status 404: the hypervisor was not found
   :status 503: one or more subsystems could not be reached








.. http:put:: /hypervisors/(uuid:hypervisor)/config

   Sets hypervisor config for hypervisor with given *uuid*.

   **Related permissions**

     hypervisors -> UUID -> edit








.. http:put:: /hypervisors/(uuid:hypervisor)/metadata[/...]

   Sets a metadata key for hypervisor with given *uuid*.

   **Related permissions**

      hypervisors -> UUID -> edit

    .. todo::
    
      Example Requests & Responses still missing.








.. http:delete:: /hypervisors/(uuid:hypervisor)/metadata/... 

    Removes a key from the metadata for hypervisor with given *uuid*.

   **Related permissions**

      hypervisors -> UUID -> edit

    **Example request**:

      .. sourcecode:: http
  
        DELETE /hypervisors/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/(path:metadata) HTTP/1.1
        host: cloud.project-fifo.net

    **Example response**:

      .. sourcecode:: http
  
        HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the metadata key was successfully deleted from the hypervisor
   :status 404: the metadata key was not found
   :status 503: one or more subsystems could not be reached







.. note::

   Characteristics are used to describe capabilities of the hypervisor for the selection process.

.. http:put:: /hypervisors/(uuid:hypervisor)/characteristics[/...]

   Sets a characteristics key for hypervisor with given *uuid*.

   **Related permissions**

      hypervisors -> UUID -> edit

    .. todo::
    
      Example Requests & Responses still missing.







.. http:delete:: /hypervisors/(uuid:hypervisor)/characteristics/...

   Removes a characteristics key for hypervisor with given *uuid*.

   **Related permissions**

      hypervisors -> UUID -> edit

    **Example request**:

      .. sourcecode:: http
  
        DELETE /hypervisors/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/characteristics/... HTTP/1.1
        host: cloud.project-fifo.net

    **Example response**:

      .. sourcecode:: http
  
        HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the characteristic was successfully removed from the hypervisor
   :status 404: the characteristic was not found
   :status 503: one or more subsystems could not be reached
