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

Tachyon has a multi layered architecture that balanaces data correctness and availability. The general premise it works under is that over all availability is at a greater importance then absolute correctness. Aside of tachyon itself two aditional components form the stack:

* The `nsq <https://nsq.io>`_ message queue, it is used to allow buffering messages in the case of outages, while buffered messages will not be visible in the database they state can later be completed.
* The `DalmatinerDB <https://dalmatiner.io>`_ database, to handle the throughput and storage requirements.
* The `Grafana <http://grafana.org>`_ dashboard, to visualize the metrics and create dashboards. (plugin required!)

At the lowest level the tachyon-meter, a slightly modified kstat, gathers statistics directly from the kernel and sends them to a nsqd process. kstat is a wonderful tool for that we easily can collect hundreds if not thousands of metrics from a system with virtually no (or minimal) impact to the system. Timestamps are generated locally (at send time) so a delayed delivery does not affect the precision, and it easiely can be correlated by other metrics generated on the system.

The Tachyon Aggregator is the second station metrics take, it decodes the kstat package and translates it into a insert statement that DalmatinerDB can understand. Those services are stateless and if the connection to DalmertinerDB is interrupted they will requeue packages to NSQ for later processing.

The data is persisted in DalmatinerDB and from there can be queried by FiFo the DalmatinerDB Frontend or Grafana.

.. toctree::
   :maxdepth: 2
   :glob:

   tachyon/installation
   tachyon/configuration
