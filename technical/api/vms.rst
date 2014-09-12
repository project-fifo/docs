.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*********
API - VMs
*********

.. http:get:: /vms

   Returns all VMs.

   **Related permissions**

    cloud -> cms -> list

   **Example request**:

      .. sourcecode:: http
  
        GET /vms HTTP/1.1
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
   :reqheader x-full-fields: fields to include in the full list - please see: :http:get:`/vms/(uuid:vm)`
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session
   
   :status 200: the VM list is returned
   :status 403: user is not authoriyed
   :status 503: one or more subsystems could not be reached







.. http:post:: /vms

   Creates a new VM.

   **Related permissions**

    cloud -> vms -> create

    .. todo::
    
      Example Requests & Responses still missing.







.. http:put:: /vms/dry_run

   Runs the VM creation in dry run.

   **Related permissions**

    cloud -> vms -> create

    .. todo::
    
      Example Requests & Responses still missing.








.. http:get:: /vms/(uuid:vm)

   Returns a VMs state for VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> get

   **Example request**:

    .. sourcecode:: http

     GET /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
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
      "alias": "fifo",
      "owner": "739faa0d-d098-496c-a87b-dc95520f8d12",

      "dataset": "e50552e8-e617-4ed3-98a6-ff5641f426f3",
      "package": "e1618837-be96-4e10-8c5f-41c223607c65",
      "hypervisor": "e57992d1-f4bc-4795-8582-5cb982a8b3ad",
      "network_map": {"192.168.0.8": "daf72785-000b-4abb-8f30-d862405d3bb2"},

      "config": {},
      "info": {},
      "services": {},
      "backups": {},
      "snapshots": {},

      "logs": [],
      "groupings": [],
      "state": "running",

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

   :>json string uuid: UUID of the VM
   :>json string alias: alias of the VM
   :>json string owner: owner of the VM

   :>json string dataset: dataset the VM is based on
   :>json string package: package of the VM
   :>json string hypervisor: hypervisor the VM runs on
   :>json object network_map: network map of the VM

   :>json object config: information about VM's config
   :>json object info: information about the VM
   :>json object services: services running on the VM
   :>json object backups: backups of the VM
   :>json object snapshots: snapshots of the VM

   :>json array logs: VM's logs
   :>json array groupings: cluster the VM is part of
   :>json string state: 'power' state of the VM

   :>json object metadata: matadate associated with the VM







.. http:put:: /vms/(uuid:vm)

   Initiates a VM state change for VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> state


   Updates the config/package for VM with given *uuid*.
     
   **Related permissions**
     
    vms -> UUID -> edit

    .. todo::
    
      Example Requests & Responses still missing.







.. http:delete:: /vms/(uuid:vm)

   Deletes VM with given *uuid* from hypervisor.

   **Related permissions**

    vms -> UUID -> delete

   **Example request**:

      .. sourcecode:: http
  
       DELETE /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the VM was successfully deleted from the hypervisor
   :status 404: the VM was not found
   :status 503: one or more subsystems could not be reached







.. http:put:: /vms/(uuid:vm)/owner

   Changes the owner of VM with given *uuid*.

   **Related permissions**

    * vms -> UUID -> edit
    * orgs -> UUID -> edit

    .. todo::
    
      Example Requests & Responses still missing.







.. http:post:: /vms/(uuid:vm)/nics

   Adds a new interface to VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> edit

    .. todo::
    
      Example Requests & Responses still missing.







.. http:put:: /vms/(uuid:vm)/nics/<mac>

   Sets an interface for VM with given *uuid* as the primary interface.

   **Related permissions**

    vms -> UUID -> edit

    .. todo::
    
      Example Requests & Responses still missing.







.. http:delete:: /vms/(uuid:vm)/nics/<mac>

   Removes a nic from the VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> edit

   **Example request**:

      .. sourcecode:: http
  
       DELETE /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/nics/<mac> HTTP/1.1
       host: cloud.project-fifo.net

    **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the nic was successfully deleted from VM
   :status 404: the nic was not found on the VM
   :status 503: one or more subsystems could not be reached








.. http:get:: /vms/(uuid:vm)/snapshots

   Lists all snapshots of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> get

   **Example request**:

    .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/snapshots HTTP/1.1
     host: cloud.project-fifo.net
     accept: applicaiton/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

    .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

      {}

    .. todo::

      Response as object has to be checked. If incorrect :json ... snapshot has to be eddited accordingly.
     

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the VM'S snapshots are returned
   :status 404: the snapshots were not found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json object snapshots: list of snapshots of the VM








.. http:post:: /vms/(uuid:vm)/snapshots

   Creates a new snapshot of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> snapshot

    .. todo::
    
      Example Requests & Responses still missing.









.. http:get:: /vms/(uuid:vm)/snapshots/(id:snapshot)

   Returns snapshot with given *ID* of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> snapshot

   **Example request**:

    .. sourcecode:: http

     GET /users/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/snapshots/c7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
     host: cloud.project-fifo.net
     accept: applicaiton/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

    .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

      {}
     

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the VM's snapshots are returned
   :status 404: the snapshot was not found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json object snapshot: list of snapshot IDs









.. http:put:: /vms/(uuid:vm)/snapshots/(id:snapshot)

   Rolls back to snapshot with given *ID* of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> rollback

    .. todo::
    
      Example Requests & Responses still missing.









.. http:delete:: /vms/(uuid:vm)/snapshots/(id:snapshot)

   Deletes snapshot with given *ID* of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> snapshot_delete

    .. todo::
    
      Example Requests & Responses still missing.








.. http:get:: /vms/(uuid:vm)/backups

   Lists all backups of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> get

    **Example request**:

      .. sourcecode:: http
  
       DELETE /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/snapshots/c7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the snapshot was successfully deleted from the VM
   :status 404: the snapshot was not found on the VM
   :status 503: one or more subsystems could not be reached









.. http:post:: /vms/(uuid:vm)/backups

   Creates a new backup of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> snapshot

    .. todo::
    
      Example Requests & Responses still missing.








.. http:get:: /vms/(uuid:vm)/backups/(id:backup)

   Returns backup with given *ID* of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> snapshot

    .. todo::
    
      Example Requests & Responses still missing.









.. http:put:: /vms/(uuid:vm)/backups/(id:backup)

   Restores backup with given *ID* of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> rollback

    .. todo::
    
      Example Requests & Responses still missing.









.. http:delete:: /vms/(uuid:vm)/backups/(id:backup)

   Deletes backup with given *ID* of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> snapshot_delete

   **Example request**:

      .. sourcecode:: http
  
       DELETE /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/backups/c7c658e0-2ddb-46dd-8973-4a59ffc9957e HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the backup was successfully deleted from the VM
   :status 404: the backup was not found on the VM
   :status 503: one or more subsystems could not be reached








.. http:put:: /vms/(uuid:vm)/metadata[/...]

   Sets a metadata key for VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> edit

    .. todo::
    
      Example Requests & Responses still missing.








.. http:delete:: /vms/(uuid:vm)/metadata/...

   Removes a metadata key for VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> edit

   **Example request**:

      .. sourcecode:: http
  
       DELETE /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/metadata/(paths:metadata) HTTP/1.1
       host: cloud.project-fifo.net

   **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the snapshot was successfully deleted from the VM
   :status 404: the snapshot was not found on the VM
   :status 503: one or more subsystems could not be reached








.. http:get:: /vms/(uuid:vm)/services

   Lists the services of a zone.

   **Related permissions**

    vms -> UUID -> get

    .. todo::
    
      Example Requests & Responses still missing.







.. http:put:: /vms/(uuid:vm)/services

   Changes state of a service on VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> edit

    .. todo::
    
      Example Requests & Responses still missing.
