.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*************
Configuration
*************

Configuration file
##################

`Sniffle <../sniffle.html>`_'s configuration file is located in ``/opt/local/sniffle/etc/sniffle.conf``. It is automatically generated on the first install and not overwritten on updates. Nonetheless the newst version of the file is always located in ``/opt/local/sniffle/etc/sniffle.conf.example``.

The configuration file is documented inline but we'll go over go over some more interesting settings here.

Active Anti Entropy (AAE)
*************************

AAE is riak's mechanism of background synchronization of systems to ensure a higher data consistency. It was ported to *FiFo* in order to offer incrased stability in multi node setups. AAE increases the required resources conciderably and does not have much use with a single system so it is **disabled by default**.

.. Attention::

  When having more then one system it is strongly recommanded to enable AAE! It is possible to selectively enable and disable for different subsystems. Generally it is OK keep it disabled for ``images`` since data does not chagne, all other systems should be switched on.

::

   # enable active anti-entropy subsystem
   anti_entropy = on
   dataset.aae = on
   hypervisor.aae = on
   vm.aae = on
   iprange.aae = on
   network.aae = on
   img.aae = off
   dtrace.aae = on
   package.aae = on

Database
********

*FiFo* uses ``leveldb`` as its backend database. Leveldb has many different tuneables some of the more important ones are.


ring_size
    The number of VNodes used by *FiFo*. This setting can only be changed before the system is booted the first time so choose it carefully. It defaults to ``8`` but with more then one systems it is very reacts very sensible the higher this number is set . Generally ``~10`` vnodes per physical node are a good rule of thumb. The ``ring_size`` needs to be a a power of two (``2``, ``4``, ``8`` ... ``64`` ...).

leveldb.mmap_size
    The chunk size of each mmaped file. This has a huge impact of the memory requirements. Since *FiFo* does not store lots of data a setting of ``1MB`` is a valid value for small and medium installations.The settings can be increased as long as it is ensured that enough memory is present.

Global configuration
####################

In addition to the config files that apply on a per node level there are global configurations that can be changed from one system and are applied globally. Unless otherwise noted all these settings can be changed during runtime.

Sniffle-admin config show
*************************

Shows a list of all settings in the global configuration.

::

      General Section
                     Key                                              Value
    -------------------- --------------------------------------------------
    storage.general.back internal
      S3 Section
                     Key                                              Value
    -------------------- --------------------------------------------------
    storage.s3.general_b                                               fifo
         storage.s3.port 8443
    storage.s3.access_ke                                                ...
    storage.s3.snapshot_                                     fifo-snapshots
         storage.s3.host                                 leofs.licenser.net
    storage.s3.secret_ke                                                ...
    storage.s3.image_buc                                        fifo-images


Sniffle-admin config set ``<key>`` ``<value>``
**********************************************

Sets a global config value please see the followign sections for valid settings.

storage.general.backend
    The backend used to store images, this can either be ``internal`` or ``s3``. This needs to be set to ``s3`` to enable backups, ``internal`` only supports storing datasets / images.

storage.s3.host
    The host for the S3 this needs to be a domainname to propperly work, the domainname either needs to resolve or be in the ``/etc/hosts``.

storage.s3.port
    The port for the S3's HTTPS interface, please note that HTTP is not supported.

storage.s3.access_key
    The access key for the S3 system.

storage.s3.secret_key
    The secret key for the S3 system.

storage.s3.general_bucket
    The bucket used for unknown data (currently not used).

storage.s3.image_bucket
    The bucket used for datasets/images.

storage.s3.snapshot_bucket
    The bucket used for backups, the bakups are stored in the format ``<vm uuid>/<backup uuid>``.
