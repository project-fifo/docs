.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

************
Installation
************

Please read the `Chunter <../chunter.html>`_ section in the current build section above for basic information on `Chunter <../chunter.html>`_ and some helpful notes before continuing.

To install `Chunter <../chunter.html>`_ we do the following:

.. note::

  - To install the relase version use *VERSION=rel*
  - To install the development version use *VERSION=dev*


.. code-block:: bash

   VERSION=rel
   cd /opt
   curl -O http://release.project-fifo.net/chunter/${VERSION}/chunter-latest.gz
   gunzip chunter-latest.gz
   sh chunter-latest

Once services have been installed, we enable the service and ensure that it is running

.. code-block:: bash

   svcadm enable epmd chunter
   svcs chunter


If the service is running you should see:

::

   STATE          STIME    FMRI
   online         Dec_31   svc:/network/chunter:default


Once the service is running *FiFO* will auto-discover the node and after about a minute the *SmartOS Node* will appear in the *FiFO* web browser to be managed.

`Chunter <../chunter.html>`_ will try to guess the systems IP. It will look for a ``fifo0`` vnic. If this doesn't exist it will take the IP of the ``admin`` nic. If `Chunter <../chunter.html>`_ registered with a wrong IP it needs to be changed in the config file and the hypervisor need to be reregistered. This can be done by `removing it <../sniffle/administration.html>`_ and restarting `Chunter <../chunter.html>`_.
