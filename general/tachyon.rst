.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

Tachyon
#######

Tachyon is a monitoring system for SmartOS (and in theory other Illumos/Solaris derivates) build to monitor large server farms and deal with the amount of data generated there. It does integrate with FiFo, however it is perfectly capable of running on it's own.

Architecture
------------

.. image:: /_static/images/tachyon.png

Tachyon has a multi layered architecture that balanaces data correctness and availability. The general premesis it works under is that over all availability is at a greater importance then absolute correctness. Aside of tachyon itself two aditional components form the stack:

* The `nsq <https://nsq.io>`_ message queue, it is used to allow buffering messages in the case of outages, while buffered messages will not be visible in the database they state can later be completed.
* The `DalmaterDB <https://dalmatiner.io>`_ database, to handle the throughput and storage requirements.
* The `Grapfana <http://grapfana.org>`_ dashboard, to visualize the metrics and create dashboards. (`plugin required!)

At the lowest level the tachyon-meter, a slightly modified kstat, gatheres statistics directly from the knernel and send them to a nsqd process. kstat is a wonderful tool for that we easiely can collect hundres if not thousands of metrics from a system with virtually no (or minimal) impact to the system. Timestamps are generated locally (at send time) so a delayed delivery does not affect the precision, and it easiely can be correlated by other metrics generated on the system.

The Tachyon Aggregator is the second station metrics take, it decodes the kstat package and translates it into a insert statement that DalmatinerDB can understand. Those services are stateless and if the connection to DalmertinerDB is interrupted they will requeue packages to NSQ for later processing.

The data is persisted in DalmatinerDB and from there can be queried by FiFo the DalmatinerDB Frontend or Grafana.


Installation
------------

NSQ
```

Installing NSQ in a zone is rather simple, Project-FiFo provides a SmartOS package that can be installed with the following command. The package includes all nsq components, the nsqd queue as well as the admin interface and the nsqadmind, all of them can be configured over the SMF configuration parameters.

.. code-block:: bash

   pkg_add http://release.project-fifo.net/pkg/rel/nsq

Another option is to install the nsqd in the GZ however that comes wiht it's own issues and at this point is not recommanded.


Tachyon Aggregator
``````````````````
