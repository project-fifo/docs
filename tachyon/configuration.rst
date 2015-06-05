.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

Configuration
#############

Tachyon Meter
-------------

The file ``/opt/tachyon-meter/etc/tachyon.conf`` needs to be edited:

.. code-block:: bash

   # The NSQd host to send data to
   host=192.168.1.41 # Needs to be changed to the IP of the zone hosting the NSQd daemon

   # The port NSQd listens to HTTP messages
   port=4151 # Does not need to be changed

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
------------------

The file ``/opt/local/tachyon/etc/tachyon.conf`` needs to be edited, most options are explained
in the file, the two most important ones are the following:

.. code-block:: bash

   ## The DalmatinerDB backend (if used).
   ##
   ## Default: 127.0.0.1:5555
   ##
   ## Acceptable values:
   ##   - an IP/port pair, e.g. 127.0.0.1:10011
   ddb = 192.168.1.42:5555 # Needs to be changed to point to one dalmatinerdb host

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


Rules
`````

The tachyon meter features a powerful rule engine that allows the routing (kstat pakage to ddb metric) to be defined
and route and decide to keep or discard metrics. The syntax rougly resambles erlang (or prolog for that matter) however
the effects are different.

Packages are passed through the rules from top to bottom, if a rule matches no further rules are checked.

In general a rule is written in the form ``<bucket>(<condition>) -> [<target metric>]``. There is a special case of
``ignore(<condition>)`` which means that a metric matching this condition is discarded and not send on.

Each rule can have one or more conditions, conditions have two forms:

- Matches: take the form ``<key> = <value>`` where key can be one of the following, most of the correspondend to the
  `kstat` field with the same name.
  - ``host`` - the id or hostname of the host the metric was send from (specified in tachyon meter
  - ``uuid`` - the uuid/id of an object, for zone this is the zoneuuid, for other values it will be picked if config/uuid is present
  - ``name``
  - ``module``
  - ``class``
  - ``key``
- Keywords: as ``gz`` - a shortcut for ``uuid = "global"``

The target metric is defined as an array, each element can either be a field as explained in the matcher or a string constant.

An example set to route sd (disk) related metrics to servers/<hostname>.'disk'.<instance> (and below) would look like this:

.. code-block:: erlang

   %%
   %% Disks
   %%
   server(gz, module = "sd") ->
     [host, "disk", instance, "metrics", key].

   server(gz, module = "sderr", key = "Hard Errors") ->
     [host, "disk", instance, "errors", "hard"].

   server(gz, module = "sderr", key = "Soft Errors") ->
     [host, "disk", instance, "errors", "soft"].

   server(gz, module = "sderr", key = "Transport Errors") ->
     [host, "disk", instance, "errors", "transport"].

   server(gz, module = "sderr", key = "Predictive Failure Analysis") ->
     [host, "disk", instance, "errors", "predicted_failures"].

   server(gz, module = "sderr", key = "Illegal Request") ->
     [host, "disk", instance, "errors", "illegal"].

NSQ
---

The NSQ config is done via the SMF configuration interface changing the configuration works like this:

.. code-block:: bash

   svccfg -s svc:/network/nsqd:default
   svc:/network/nsqd:default> addpg application application
   svc:/network/nsqd:default> setprop application/lookupd-tcp-address="127.0.0.1:4160"
   svc:/network/nsqd:default> refresh


The same applies for nsqadmin and nsqlookupd instances. The available configuration parameters can be
read via: ``svccfg export nsqd | grep propval``.

DalmatinerDB
------------

Please follow the `official guide <https://docs.dalmatiner.io>`_.

Grafana
-------

It mostly configured over the web interface, oterhwise see the offical documentation.
