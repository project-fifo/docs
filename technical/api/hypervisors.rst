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
