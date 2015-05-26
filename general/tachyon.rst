.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

Tachyon
#######

Tachyon is a monitoring system for SmartOS (and in theory other Illumos/Solaris derivates) build to monitor large server farms and deal with the amount of data generated there. It does integrate with FiFo, however it is perfectly capable of running on it's own.

Features
--------

* Minimal impact by using kstat directly.
* Resillient to network or component failure.
* Low and predictable space usage ~12bit / metric / second.
* Fast query times (1 or 2 digest milliseconds).
* High throughput and near linear scaling.
* No modification of NGZ's required, new zones will automatically be picked up.

Architecture
------------

.. image:: /_static/images/tachyon.png

Tachyon has a multi layered architecture that balanaces data correctness and availability. The general premesis it works under is that over all availability is at a greater importance then absolute correctness. Aside of tachyon itself two aditional components form the stack:

* The `nsq <https://nsq.io>`_ message queue, it is used to allow buffering messages in the case of outages, while buffered messages will not be visible in the database they state can later be completed.
* The `DalmaterDB <https://dalmatiner.io>`_ database, to handle the throughput and storage requirements.
* The `Grapfana <http://grapfana.org>`_ dashboard, to visualize the metrics and create dashboards. (plugin required!)

At the lowest level the tachyon-meter, a slightly modified kstat, gatheres statistics directly from the knernel and send them to a nsqd process. kstat is a wonderful tool for that we easiely can collect hundres if not thousands of metrics from a system with virtually no (or minimal) impact to the system. Timestamps are generated locally (at send time) so a delayed delivery does not affect the precision, and it easiely can be correlated by other metrics generated on the system.

The Tachyon Aggregator is the second station metrics take, it decodes the kstat package and translates it into a insert statement that DalmatinerDB can understand. Those services are stateless and if the connection to DalmertinerDB is interrupted they will requeue packages to NSQ for later processing.

The data is persisted in DalmatinerDB and from there can be queried by FiFo the DalmatinerDB Frontend or Grafana.


Installation
------------


Tachyon Meter
`````````````
The tachyon meter can be downloaded from http://release.project-fifo.net/chunter/dev/ and installed the same way as chunter. Ther is a config file in which the IP of the nsqd process has to be specified.

.. code-block:: bash

   cd /opt
   curl -O http://release.project-fifo.net/chunter/dev/tachyon-meter-latest.gz
   gunzip tachyon-meter-latest.gz
   sh tachyon-meter-latest

NSQ
```

Installing NSQ in a zone is rather simple, Project-FiFo provides a SmartOS package that can be installed with the following command. The package includes all nsq components, the nsqd queue as well as the admin interface and the nsqadmind, all of them can be configured over the SMF configuration parameters.

.. code-block:: bash

   pkg_add http://release.project-fifo.net/pkg/rel/nsq
 
Another option is to install the nsqd in the GZ however that comes wiht it's own issues and at this point is not recommanded.


Tachyon Aggregator
``````````````````

.. note::
   currently only in the dev repository.

The aggregator can be installed out of the Project FiFo package repository, either via ``pkg_add`` or via pkgin install if the repository was added to the dependenceis. The package name is tahcyon-aggregator


DalmatinerDB
````````````

Please follow the `official guide <https://docs.dalmatiner.io>`_.


Grafana
```````

There is currently no SmartOS package for Grafna2, it requires manual complication, you can follow the installation guide. Since DalmatinerDB does not ship as a native datasourece we maintain a `fork <https://github.com/dalmatinerdb/grafana>`_.

Once installed you can add DalmaterDB to the dependecnies, the default port of the Dalmaterin Frontend server is 8080.

For the time being a precompiled binary of grafana with our changes can be downloaded `here <http://release.project-fifo.net/chunter/rel/grafana-2.0.2.tgz>`_.

Configuration
-------------

Tachyon Meter
`````````````

The file ``/opt/tachyon-meter/etc/tachyon.conf`` needs to be edited:

.. code-block:: bash

   # The NSQd host to send data to
   host=192.168.1.41 # Needsto be changed to the IP of the zone hosting the NSQd deamon

   # The port NSQd listens to HTTP messages
   port=4151 # Does not need to be cahnged

   # Tne NSQ topic to send to
   topic=tachyon # Does not need to be changed

   # The interval to send data to NSQ to in seconds
   interval=1 # does not need to be changed

   # The hostname to identify the server with
   ## Will try to pick up chunters host_id file if existing otherwise
   ## simply use the hostname.
   if [ -f /opt/chunter/etc/host_id ]
   then
     hostname="$(cat /opt/chunter/etc/host_id)"
   else
     hostname="$(hostname)"
   fi

   is_smf=yes # Does not need to be changed, required for backgrouding in the SMF

Tachyon Aggregator
``````````````````

The file ``/opt/local/tachyon/etc/tachyon.conf`` needs to be edited, most options are explained
in the file, the two most important ones are the following:

.. code-block:: bash

   ## The DalmatinerDB backend (if used).
   ##
   ## Default: 127.0.0.1:5555
   ##
   ## Acceptable values:
   ##   - an IP/port pair, e.g. 127.0.0.1:10011
   ddb = 192.168.1.42:5555 # Needs to be chainged to point to one dalmatinerdb host

   ## One more more nsqlookupd http interfaces for tachyon to discover
   ## the channels.
   ##
   ## Default: 127.0.0.1:4161
   ##
   ## Acceptable values:
   ##   - an IP/port pair, e.g. 127.0.0.1:10011
   nsqlookupd.name = 127.0.0.1:4161 # Neds to be pointed to a nsq lookup deamon,
                                    # more then one of this can be used with
                                    # different names


NSQ
```

The NSQ config is done via the SMF confingration interface changing the configuration works liek this:

.. code-block:: bash

   svccfg -s svc:/network/nsqd:default
   svc:/network/nsqd:default> addpg application application
   svc:/network/nsqd:default> setprop application/lookupd-tcp-address="127.0.0.1:4160"
   svc:/network/nsqd:default> refresh


The same applies for nsqadmin and nsqlookupd instances. The available configuration parameters can be
read via: ``svccfg export nsqd | grep propval``.

DalmatinerDB
````````````

Please follow the `official guide <https://docs.dalmatiner.io>`_.

Grafana
```````

It mostly configured over the web interface, oterhwise see the offical documentation.
