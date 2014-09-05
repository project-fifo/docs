.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*********
API - VMs
*********

.. http:get:: /vms

   Returns all VMs.

   **Related permissions**

   cloud -> cms -> list


.. http:post:: /vms

   Creates a new VM.

   **Related permissions**

   cloud -> vms -> create


.. http:put:: /vms/dry_run

   Runs the VM creation in dry run.

   **Related permissions**

   cloud -> vms -> create


.. http:get:: /vms/(uuid:vm)

   Returns a VMs state for VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> get

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
      "alias": "",
      "owner": "",

      "dataset": "",
      "package": "",
      "hypervisor": "",
      "network_map": {},

      "config": {},
      "info": {},
      "services": {},
      "backups": {},
      "snapshots": {},

      "logs": [],
      "groupings": [],
      "state": "",

      "metadata": {}
     }

   :reqheader accept: the accepted encoding, valid is ``application/json``
   :resheader content-type: the returned datatype, usually ``application/json``
   :resheader x-snarl-token: the snarl token for this session

   :status 200: the session information is returned
   :status 403: user is not authoriyed
   :status 404: the session was not found
   :status 503: one or more subsystems could not be reached


.. http:put:: /vms/(uuid:vm)

   Initiates a VM state change for VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> state


   Updates the config/package for VM with given *uuid*.
     
   **Related permissions**
     
   vms -> UUID -> edit


.. http:delete:: /vms/(uuid:vm)

   Deletes VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> delete

   Deletes VM with given *uuid* from hypervisor.

   **Related permissions**

   vms -> UUID -> delete


.. http:put:: /vms/(uuid:vm)/owner

   Changes the owner of VM with given *uuid*.

   **Related permissions**

   * vms -> UUID -> edit
   * orgs -> UUID -> edit


.. http:post:: /vms/(uuid:vm)/nics

   Adds a new interface to VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> edit


.. http:put:: /vms/(uuid:vm)/nics/<mac>

   Sets an interface for VM with given *uuid* as the primary interface.

   **Related permissions**

   vms -> UUID -> edit


.. http:delete:: /vms/(uuid:vm)/nics/<mac>

   Removes a nic from the VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> edit


.. http:get:: /vms/(uuid:vm)/snapshots

   Lists all snapshots of VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> get


.. http:post:: /vms/(uuid:vm)/snapshots

   Creates a new snapshot of VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> snapshot


.. http:get:: /vms/(uuid:vm)/snapshots/(id:snapshot)

   Returns snapshot with given *ID* of VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> snapshot


.. http:put:: /vms/(uuid:vm)/snapshots/(id:snapshot)

   Rolls back to snapshot with given *ID* of VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> rollback


.. http:delete:: /vms/(uuid:vm)/snapshots/(id:snapshot)

   Deletes snapshot with given *ID* of VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> snapshot_delete

.. http:get:: /vms/(uuid:vm)/backups

   Lists all backups of VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> get


.. http:post:: /vms/(uuid:vm)/backups

   Creates a new backup of VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> snapshot


.. http:get:: /vms/(uuid:vm)/backups/(id:backup)

   Returns backup with given *ID* of VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> snapshot


.. http:put:: /vms/(uuid:vm)/backups/(id:backup)

   Restores backup with given *ID* of VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> rollback


.. http:delete:: /vms/(uuid:vm)/backups/(id:backup)

   Deletes backup with given *ID* of VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> snapshot_delete

.. http:put:: /vms/(uuid:vm)/metadata[/...]

   Sets a metadata key for VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> edit


.. http:delete:: /vms/(uuid:vm)/metadata/...

   Removes a metadata key for VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> edit


.. http:get:: /vms/(uuid:vm)/services

   Lists the services of a zone.

   **Related permissions**

   vms -> UUID -> get

.. http:put:: /vms/(uuid:vm)/services

   Changes state of a service on VM with given *uuid*.

   **Related permissions**

   vms -> UUID -> edit
