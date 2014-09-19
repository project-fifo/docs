.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*************
Configuration
*************

Configuration file
##################

`Snarl's <../snarl.html>`_ configuration file is located in ``/opt/local/fifo-snarl/etc/snarl.conf``. It is automatically generated on the first install and will not be overwritten on updates. Nonetheless the newst version of the file is always located in ``/opt/local/snarl/etc/snarl.conf.example``.

The configuration file is documented in-line but we'll go over some more interesting settings here.

Active Anti Entropy (AAE)
*************************

AAE is riak's mechanism of background synchronization of systems to ensure a higher data consistency. It was ported to FiFo to offer increased stability in multi node setups. AAE increases the required resources considerably and does not have much use with a single system so it is **disabled by default**.

.. attention::

    When having more then one system it strongly recommended to enable AAE!

::

    ## enable active anti-entropy subsystem
    anti_entropy = on


Database
********

*FiFo* uses *LevelDB* as it's back-end database. LevelDB has many different tune-ables. Some of the more important ones are:

``ring_size``
    The number of VNodes used by FiFo, this setting can only be changed before the system is booted the first time so choose it carefully, it defaults to ``8`` but with more then one systems it is very sensible to increase this number higher, generally ``~10`` vnodes per physical node are a good rule of thumb. The ``ring_size`` needs to be a a power of two (``2``, ``4``, ``8`` ... ``64`` ...).

``leveldb.mmap_size``
    The chunk size of each mmaped file, this has a huge impact of the memory requirements. Since FiFo does not store lots of data a setting of ``1MB`` is a valid value for small and medium installations, the settings can be increased as long as it is ensured that enough memory is present.

Multi DC support
****************

The mutli DC support in FiFo is based in `Snarl <../snarl.html>`_. It works by synchronizing user data between all data-centers. Synchronization happens on a point to point system meaning that each `Snarl <../snarl.html>`_ instance in ``DC1`` synchronizes with one system in ``DC2``.

By default the synchronization subsystem is disabled. It needs to be enabled and configured in order to properly propagate data.

``sync``
    Either set to ``on`` or ``off`` enabling or disabling the system, a restart is required to enable it but it can be changed at any time after the installation.

``sync.ip``
    The IP/Port this server will listen to for incoming sync connections. The syntax is ``<IP>:<Port>``, this needs to be matched on the remote side.

``sync.partner.$name``
    One Sniffle node is able to sync to multiple remote nodes, usually it should have exactly one parter per data-center, having multiple partners in the same data-center is unsupported can have unforeseen results. The syntax is ``<IP>:<Port>``.

``sync.build_interval``
    For each subsystem Snarl keeps a list of hashes, the ``build_interval`` determines how often the system rebuilds this from scratch. Aside of the full rebuild the hashes are dynamically updated.

``sync.read_delay``
    During a full rebuild reads from the Snarl system are delayed to prevent slowing down the database during the rebuild this value specifies the delay between reads.

``sync.interval``
    This defines how often it should be checked if differences exists between local and remote nodes. Please keep in mind that sync should be set up in both directions this syncs will be performed about twice as often as specified here.

``sync.recv_timeout``
    If a command is send to a sync partner and this threshold is exceeded the connection will attempt to bounce and be reestablished.


`Yubico <https://yubico.com/>`_
********************************

*FiFo* support two factor authentication with Yubikeys, to enable this `Snarl <../snarl.html>`_ needs to be set up a API key with the following settings:

``yubico.client_id``
    This section deals with Yubikey support. An API key and client ID can be obtained from https://upgrade.yubico.com/getapikey/.

``yubico.secret_key``
    The secret key related that was issued along with the Yubico client ID.

Global configuration
####################

In addition to the config files that apply on a per node level there are global configurations that can be changed from one system and are applied globally. Unless otherwise noted all those settings can be changed during runtime.

snarl-admin config show
    Shows a list of all settings in the global configuration.

snarl-admin config set ``key`` ``value``
    Sets a global config value please see the following sections for valid settings.

defaults.users.inital_role
    If this is set every user created gets placed into the role given. The value must be the UUID of the role.

defaults.users.inital_org
    If this is set every user created gets placed into the organisation given. The value must be the UUID of the organisation.

yubico.api.client_id
    When two factor authentication is wanted this must be set to the yubico API client ID.

yubico.api.secret_key
    When two factor authentication is wanted this must be set to the yubico API secret key.
