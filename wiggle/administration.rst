.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**************
Administration
**************

The `Wiggle <../wiggle.html>`_ admin command is ``wiggle-admin``.

General managemnet
##################

`Wiggle <../wiggle.html>`_ uses the SMF to manage its running state so it is restarted in the case of crashes and booted accordingly on system start. `Wiggle <../wiggle.html>`_ can be enabled, disabled and restaerted via: ``svcadm enable wiggle``, ``svcadm disable wiggle`` and ``svcadm restart wiggle``

Updating
********

`Wiggle <../wiggle.html>`_ can be updated by three simple steps.

Installing the new package
**************************

.. code-block:: bash

   pkgin -fy update
   pkgin -fy install fifo-wiggle

Updating the config
*******************

After the newest package is installed the config file should be checked for changes and eddited if needed. The ``.example`` file will always contain the newest version of the config. ``diff`` is a handy tool to see if some settings need to be added to the existing file.

.. code-block:: bash

   diff /opt/loca/fifo-wiggle/etc/wiggle.conf /opt/loca/fifo-wiggle/etc/wiggle.conf.example
   vi /opt/loca/fifo-wiggle/etc/wiggle.conf


Restarting the service
**********************
After the config is updated the service needs to be restarted. `Wiggle <../wiggle.html>`_ is running clustered and has more then ``N`` it is often possible to do a rolling update by restarting one by one.
