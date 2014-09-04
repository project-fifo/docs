.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*******************
API - Organizations
*******************

.. http:get:: /orgs

   Lists all organizations.

   **Related permissions**

   cloud -> orgs -> list 


.. http:post:: /orgs

   Creates a new organization.

   **Related permissions**

   cloud -> orgs -> create


.. http:get:: /orgs/(uuid:org)

   Returns organization with given *uuid*.

   **Related permissions**

   orgs -> UUID -> get


.. http:delete:: /orgs/(uuid:orgs)

   Deletes organization with given *uuid*.

   **Related permissions**

   orgs -> UUID -> delete
      

.. http:get:: /orgs/(uuid:org)/triggers

   Lists the organizations triggers.

   **Related permissions**

   orgs -> ID -> get


.. http:put:: /orgs/(uuid:org)/triggers/(uuid:role)/<permission.../...>

   Adds a new trigger to org with given *uuid*.

   **Related permissions**

   * orgs -> UUID -> edit
   * roles -> ROLE -> grant


.. http:delete:: /orgs/(uuid:org)/triggers/(uuid:role)/<permission.../...>

   Deletes a trigger from organization with given *uuid*.

   **Related permissions**

   orgs -> UUID -> edit


.. http:put:: /orgs/(uuid:org)/metadata[/...]

   Sets a metadata key for organization with given *uuid*.

   **Related permissions**

   orgs -> UUID -> edit


.. http:delete:: /orgs/(uuid:org)/metadata/...

   Removes a key from the metadata for organization with given *uuid*.

   **Related permissions**

   orgs -> UUID -> edit
