.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

Snarl
#####

Snarl is Project FiFo's RBAC service. Snarl allows for fine grained RBAC style permissions.   

Snarl has multi-datacenter that allows for synchronization of user data between multiple datacenters. 

Snarl also has multi form factor authentication support through the use of Yubikeys. 

Snarl has the following properties:

* No SPOF when using multiple nodes.
* Data is stored using LevelDB.
* Provides authorization and authentication for user actions inside Project-FiFo and Project-FiFo controlled vms.


*Snarl Section Topics:*

.. toctree::
   :maxdepth: 2
   :glob:

   snarl/configuration
   snarl/administration
   snarl/permissions
