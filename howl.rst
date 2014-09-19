.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

Howl
####

Howl is Project FiFo's live communications service. Howl live updates the user interface (Jingles)
when changes occur in the back-end. Howl checks with snarl (Project FiFo's RBAC service) for which
events a user has permission to see. 

Howl is a riak_core and web-socket based message delivery system with the following properties:

* No SPOF when using multiple nodes.
* The front end is a cowboy web server that accepts web socket connections.
* The back-end is a simple TPC/BERT based protocol.
* Each riak_core node offers both a front end and a back-end, those front and back ends are equal, as in sending messages to any back-end will reach any front end.
* Messages are send to channels, and clients can subscribe to channels.


*Howl Section Topics:*


.. toctree::
   :maxdepth: 2
   :glob:

   howl/configuration
   howl/administration
   howl/permissions
