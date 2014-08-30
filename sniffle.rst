.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

Sniffle
#######

Sniffle is Project FiFo's central decision making and datastore service. Sniffle stores data about hypervisors, vms, networks, IP ranges, and packages. Besides being the central datastore, Sniffle also makes system wide decisions such as vm placement. 

Sniffle has the following properties:

* No SPOF when using multiple nodes.
* Data is stored using LevelDB.
* Orders actions that take place on Chunter (Project FiFo's hypervisor interface).
* Communicates changes to Howl (Project FiFo's live communication service) for realtime updates (ie. vm boots, reboots).
* Commands Snarl (Project FiFo's RBAC service) to grant/revoke rights, and checks for permissions before taking action.


*Sniffle Section Topics:*

.. toctree::
   :maxdepth: 2
   :glob:

   sniffle/configuration
   sniffle/administration
   sniffle/permissions