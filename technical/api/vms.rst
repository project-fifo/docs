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

____







.. http:post:: /vms

   Creates a new VM.

   **Related permissions**

    cloud -> vms -> create

    POST /api/0.1.0/vms HTTP/1.1
    Accept: application/json
    Content-Type: application/json
    x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd

    {"package":"aa77ce44-cdb6-4e59-8f64-97f65b7eba2d","dataset":"d34c301e-10c3-11e4-9b79-5f67ca448df0","config":{"networks":{"net0":"a3850354-d356-4bb7-a9ae-a41387702ad5"},"metadata":{},"alias":"base64","hostname":"base64","requirements":[],"autoboot":true}}

    HTTP/1.1 303 See Other
    Content-Type: application/json
    vary: accept
    location: /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab


.. todo::

  Example Requests & Responses still missing.

____







.. http:put:: /vms/dry_run

   Runs the VM creation in dry run.

   **Related permissions**

    cloud -> vms -> create

    POST /api/0.1.0/vms/dry_run HTTP/1.1
    Accept: application/json
    Content-Type: application/json
    x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd

    {"package":"aa77ce44-cdb6-4e59-8f64-97f65b7eba2d","dataset":"d34c301e-10c3-11e4-9b79-5f67ca448df0","config":{"networks":{"net0":"a3850354-d356-4bb7-a9ae-a41387702ad5"},"metadata":{},"alias":"base64","hostname":"base64","requirements":[],"autoboot":true}}
     HTTP/1.1 201 Created
     Content-Type: application/json
     x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
     vary: accept


.. todo::

  Example Requests & Responses still missing.

____








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

____







.. http:put:: /vms/(uuid:vm)

   Initiates a VM state change for VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> state

    .. warning there are two examples for get requests since this endpoint can take different data and act differntly


    PUT /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab HTTP/1.1
    Accept: application/json
    x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
    Content-Type: application/json

    {"action":"stop"}


    PUT /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab HTTP/1.1
    Accept: application/json
    x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
    Content-Type: application/json

    {
     "config":{"alias":"alias","hostname":"base64","resolvers":["8.8.8.8","8.8.4.4"]},
     "package":"356574be-28ba-4e11-8073-166b3ea278a0"
    }


    HTTP/1.1 204 No Content
    Content-Type: application/json
    x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
    vary: accept

   Updates the config/package for VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> edit

.. todo::

  Example Requests & Responses still missing.

____







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

____







.. http:put:: /vms/(uuid:vm)/owner

   Changes the owner of VM with given *uuid*.

   **Related permissions**

    * vms -> UUID -> edit
    * orgs -> UUID -> edit

   PUT /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/owner HTTP/1.1
   accept: application/json
   origin: http://192.168.221.201
   x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
   content-type: application/json

   {"org":"63952b63-a42f-4649-8cbb-c951724faf2b"}


   HTTP/1.1 204 No Content
   vary: accept

.. todo::

  Example Requests & Responses still missing.

____







.. http:post:: /vms/(uuid:vm)/nics

   Adds a new interface to VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> edit

    POST /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/nics HTTP/1.1
    Accept: application/json, text/plain, */*
    Content-Type: application/json;charset=UTF-8
    x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd

    {"network":"a3850354-d356-4bb7-a9ae-a41387702ad"5}

    HTTP/1.1 303 See Other
    Content-Type: application/json
    vary: accept
    location: /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab

.. todo::

  Example Requests & Responses still missing.

____







.. http:put:: /vms/(uuid:vm)/nics/(mac: nic)

   Sets an interface for VM with given *uuid* as the primary interface.

   **Related permissions**

    vms -> UUID -> edit

   PUT /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/nics/d2:1f:b4:36:47:e2 HTTP/1.1
   Accept: application/json
   x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd

   {"primary":true}

   HTTP/1.1 204 No Content
   vary: accept


.. todo::

  Example Requests & Responses still missing.

____







.. http:delete:: /vms/(uuid:vm)/nics/(mac: nic)

   Removes a nic from the VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> edit

   **Example request**:

      .. sourcecode:: http

       DELETE /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/nics/(mac: nic) HTTP/1.1
       host: cloud.project-fifo.net

    **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the nic was successfully deleted from VM
   :status 404: the nic was not found on the VM
   :status 503: one or more subsystems could not be reached

____








.. http:get:: /vms/(uuid:vm)/snapshots

   Lists all snapshots of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> get

   **Example request**:

    .. sourcecode:: http

     GET /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/snapshots HTTP/1.1
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

   :status 200: the VM'S snapshots are returned
   :status 404: no snapshots were found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json object snapshots: list of snapshots of the VM

.. todo::

  Response as object has to be checked. If incorrect :json ... backup has to be eddited accordingly.

____








.. http:post:: /vms/(uuid:vm)/snapshots

   Creates a new snapshot of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> snapshot

.. todo::
    
  Example Requests & Responses still missing.

____









.. http:get:: /vms/(uuid:vm)/snapshots/(id:snapshot)

   Returns snapshot with given *ID* of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> snapshot

   **Example request**:

    .. sourcecode:: http

     GET /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/snapshots/917c56d4-3a33-11e4-84fa-0be1f7e1f583 HTTP/1.1
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

   :status 200: information about the snapshot is returned
   :status 404: the snapshot was not found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json object snapshot: data still missing


.. todo:: 

  * Response as object has to be checked. If incorrect :>json ... snapshot has to be eddited accordingly.

  * data has to be added for :>json object snapshot

____










.. http:put:: /vms/(uuid:vm)/snapshots/(id:snapshot)

   Rolls back to snapshot with given *ID* of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> rollback

.. todo::
    
  Example Requests & Responses still missing.

____









.. http:delete:: /vms/(uuid:vm)/snapshots/(id:snapshot)

   Deletes snapshot with given *ID* of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> snapshot_delete

   **Example request**:

      .. sourcecode:: http
  
       DELETE /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/snapshots/9157369c-3a33-11e4-bdc5-63dd38248522 HTTP/1.1
       host: cloud.project-fifo.net

    **Example response**:

      .. sourcecode:: http
  
       HTTP/1.1 204 No Content

   :reqheader x-snarl-token: the snarl token for this session
   :resheader x-snarl-token: the snarl token for this session

   :status 204: the snapshot was successfully deleted from VM
   :status 404: the snapshot was not found on the VM
   :status 503: one or more subsystems could not be reached

____








.. http:get:: /vms/(uuid:vm)/backups

   Lists all backups of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> get

   **Example request**:

    .. sourcecode:: http

     GET /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/backups HTTP/1.1
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

   :status 200: the VM's backups are returned
   :status 404: no backups were found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json object backups: list of backups of the VM

.. todo::

 Response as object has to be checked. If incorrect :json ... backup has to be eddited accordingly.

____









.. http:post:: /vms/(uuid:vm)/backups

   Creates a new backup of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> snapshot

    POST /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/backups HTTP/1.1
    Accept: application/json
    Content-Type: application/json

    {"comment":"initial"}

    HTTP/1.1 303 See Other
    Content-Type: application/json
    vary: accept
    location: /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/backups/e7ae7ad3-686e-4eef-8478-c289b254824b

.. todo::
    
  Example Requests & Responses still missing.


.. http:get:: /vms/(uuid:vm)/backups/(id:backup)

   Returns backup with given *ID* of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> snapshot

   **Example request**:

    .. sourcecode:: http

     GET /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/backup/917cc81c-3a33-11e4-91be-d75626cf1357 HTTP/1.1
     host: cloud.project-fifo.net
     accept: applicaiton/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

   **Example response**:

    .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json
     x-snarl-token: 1b2230af-03bb-4bf7-ab49-86fab503bf16

     {"comment":"initial","pending":true,"timestamp":1410571703253652,"uuid":"e7ae7ad3-686e-4eef-8478-c289b254824b"}

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :reqheader x-snarl-token: the snarl token for this session
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: information about the backup is returned
   :status 404: the backup was not found
   :status 403: user is not authorized
   :status 503: one or more subsystems could not be reached

   :>json object backup: data still missing

.. todo::

  * Response as object has to be checked. If incorrect :>json ... backup has to be eddited accordingly.

  * data has to be added for :>json object backup

____








.. http:put:: /vms/(uuid:vm)/backups/(id:backup)

   Restores backup with given *ID* of VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> rollback

  PUT /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/backups/e7ae7ad3-686e-4eef-8478-c289b254824b HTTP/1.1
  Accept: application/json
  x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
  Content-Type: application/json

  {"action":"rollback"}

  HTTP/1.1 204 No Content
  Content-Type: application/json
  x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
  vary: accept


.. todo::

  Example Requests & Responses still missing.

____









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

____








.. http:put:: /vms/(uuid:vm)/metadata[/...]

   Sets a metadata key for VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> edit

    PUT /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/metadata/jingles HTTP/1.1
    Accept: application/json
    x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
    Content-Type: application/json

    {"notes":[{"text":"yap","created_at":"2014-09-13T01:34:03.379Z"}]}


    HTTP/1.1 204 No Content
    Content-Type: application/json
    x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
    vary: accept

.. todo::

  Example Requests & Responses still missing.

____








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

____








.. http:get:: /vms/(uuid:vm)/services

   Lists the services of a zone.

   **Related permissions**

    vms -> UUID -> get

   **Example request**:

    .. sourcecode:: http

     GET /vms/b7c658e0-2ddb-46dd-8973-4a59ffc9957e/services HTTP/1.1
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

  :status 200: the VM's services are returned
  :status 404: no services were found
  :status 403: user is not authorized
  :status 503: one or more subsystems could not be reached

  :>json object services: 

.. todo::

  * Response as object has to be checked. If incorrect :json ... services has to be eddited accordingly.

  * data has to be added for :json object services

____







.. http:put:: /vms/(uuid:vm)/services

   Changes state of a service on VM with given *uuid*.

   **Related permissions**

    vms -> UUID -> edit

    PUT /api/0.1.0/vms/2ca285a3-05a8-4ca6-befd-78fa994929ab/services HTTP/1.1
    Accept: application/json, text/plain, */*
    x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
    Content-Type: application/json;charset=UTF-8

    {"action":"disable", "service":"svc:/system/svc/restarter:default"}

    HTTP/1.1 204 No Content
    Content-Type: application/json
    x-snarl-token: d2d685b7-714d-4d28-bb7c-6f80b29da4dd
    vary: accept

.. todo::

  Example Requests & Responses still missing.

