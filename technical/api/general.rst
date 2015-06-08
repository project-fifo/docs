.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*************
API - General
*************

.. note::

   The base path of all PI calls is: ``/api/<version>`` where ``<version>`` is
   currently ``0.1.0`` but might change on a later date if breaking changes are
   introduced.


.. http:get:: /cloud/connection

  Returns the connection status.

  **Example request**:

  .. sourcecode:: http

   GET /cloud/connection HTTP/1.1
   host: cloud.project-fifo.net
   accept: application/json

  **Example response**:

  .. sourcecode:: http

   HTTP/1.1 200 OK
   vary: Accept
   content-type: application/json

    {
      "howl": 1,
      "snarl": 1,
      "sniffle": 1
    }

  :reqheader accept: the accepted encoding, valid is ``application/json``
  :resheader content-type: the returned datatype, usually ``application/json``

  :status 200: The system is operational.
  :status 503: One or more subsystems could not be reached.

  :>json integer howl: number of connected howl servers
  :>json integer sniffle: number of connected sniffle servers
  :>json integer snarl: number of connected snarl servers

____


.. http:get:: /cloud

   Retrieve the general cloud status.

   **Related permissions**

		cloud -> cloud -> status

   **Example request**:

   .. sourcecode:: http

     GET /cloud HTTP/1.1
     host: cloud.project-fifo.net
     accept: application/json
     Authorization: Bearer gjGGIkIM2m518n4UmEgubIH0H2Xkt1Y6

   **Example response**:

   .. sourcecode:: http

     HTTP/1.1 200 OK
     vary: Accept
     content-type: application/json

      {
        "warnings": [],
        "metrics": {
          "users": 0,
          "roles": 0,
          "orgs": 0,
          "used": 99121,
          "total-memory": 65508,
          "storage": "s3",
          "size": 454656,
          "reserved-memory": 0.0,
          "provisioned-memory": 46080,
          "l2size": 0,
          "l2miss": 0,
          "l2hits": 0,
          "l1size": 1830,
          "l1miss": 831913,
          "l1hits": 24159020,
          "hypervisors": [
            "cae242d0-fb7a-4a37-82c7-dcc73ce0fa8d"
          ],
          "free-memory": 19428.0,
          "vms": 6
        },
        "versions": {
          "snarl": "test-a895db8, Tue Aug 26 10:40:50 2014 -0400",
          "howl": "test-1b05ae8, Sat Jul 19 02:52:51 2014 +0200",
          "sniffle": "test-a3eeb09, Mon Aug 25 16:58:50 2014 -0400",
          "wiggle": "test-31008f1, Tue Aug 26 12:12:16 2014 -0400"
        }
      }

   :reqheader accept: accepted encoding, valid is ``application/json``
   :reqheader authorization: Bearer token for OAuth2 auth
   :resheader content-type: returned datatype, usually ``application/json``

   :status 200: the system is operational
   :status 403: the logged in user lacks the needed permissions
   :status 503: one or more subsystems could not be reached

   :>json array warnings: list containing bad system states
   :>json object metrics: information on various system metrics
   :>json integer users: number of users on the system
   :>json integer roles: number of roles on the system
   :>json integer orgs: number of organization on the system
   :>json integer used:
   :>json integer total-memory: total memory available to the system
   :>json string storage:
   :>json integer size:
   :>json integer reserved-memory:
   :>json integer l2size: size of the level 2 arc
   :>json integer l2miss:
   :>json integer l2its:
   :>json integer l1size: size of the level 1 arc
   :>json integer l1miss:
   :>json integer l1hits:
   :>json array hypervisors: the hypervisor's UUID
   :>json integer free memory:
   :>json integer VMS: total amount of VMs available

   :>json object versions: versions of the system components
   :>json string snarl: snarl version
   :>json string howl: howl version
   :>json string sniffle: sniffle version
   :>json string wiggle: wiggle version
