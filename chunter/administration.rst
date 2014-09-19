.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
Administration
**************

General management
##################

`Chunter <../chunter.html>`_ uses the SMF to manage it's running state so it is restarted in the case of crashes and booted accordingly on system start. Chunter can be enabled, disabled and restarted via: ``svcadm enable chunter``, ``svcadm disable chunter`` and ``svcadm restart chunter``

Updating
********

Chunter can be updated by running ``/opt/chunter/bin/update`` this script will check for new updates and install them as needed.

Installing the new package
**************************

.. code-block:: bash

   /opt/chunter/bin/update

Updating the config
*******************

After the newest package is installed the config file should be checked for changes and edited if needed. The ``.example`` file will always contain the newest version of the config. ``diff`` is a handy tool to see if some settings need to be added to the existing file.

.. code-block:: bash

   diff /opt/chunter/etc/chunter.conf /opt/chunter/etc/chunter.conf.example
   vi /opt/chunter/etc/chunter.conf

Restarting the service
**********************

Chunter can be restarted by running ``svcadm restart chunter``.

Manually created VM's
*********************

Chunter does not automatically detect manually created VMs. To make Chunter aware of them it needs to be restarted: ``svcadm restart chunter``.
