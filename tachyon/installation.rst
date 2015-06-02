.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

Installation
############


Tachyon Meter
-------------

The tachyon meter can be downloaded from http://release.project-fifo.net/chunter/dev/ and installed the same way as chunter. Ther is a config file in which the IP of the nsqd process has to be specified.

.. code-block:: bash

   cd /opt
   curl -O http://release.project-fifo.net/chunter/dev/tachyon-meter-latest.gz
   gunzip tachyon-meter-latest.gz
   sh tachyon-meter-latest

NSQ
---

Installing NSQ in a zone is rather simple, Project-FiFo provides a SmartOS package that can be installed with the following command. The package includes all nsq components, the nsqd queue as well as the admin interface and the nsqadmind, all of them can be configured over the SMF configuration parameters.

.. code-block:: bash

   pkg_add http://release.project-fifo.net/pkg/rel/nsq
 
Another option is to install the nsqd in the GZ however that comes wiht it's own issues and at this point is not recommended.


Tachyon Aggregator
------------------

.. note::
   currently only in the dev repository.

The aggregator can be installed out of the Project FiFo package repository, either via ``pkg_add`` or via pkgin install if the repository was added to the dependencies. The package name is tahcyon-aggregator


DalmatinerDB
------------

Please follow the `official guide <https://docs.dalmatiner.io>`_.


Grafana
-------

There is currently no SmartOS package for Grafana2, it requires manual compilation, you can follow the installation guide. Since DalmatinerDB does not ship as a native datasource we maintain a `fork <https://github.com/dalmatinerdb/grafana>`_.

Once installed you can add DalmatinerDB to the dependencies, the default port of the Dalmatiner Frontend server is 8080.

For the time being, a precompiled binary of grafana with our changes can be downloaded `here <http://release.project-fifo.net/chunter/rel/grafana-2.0.2.tgz>`_.
