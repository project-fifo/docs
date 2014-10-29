.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

Watchdog
########

Watchdog is introduced with FiFo **0.6.1** earlyer releases do not have support for it!

Architecture
------------

Most of watchdog runs inside the cloud's network with the optional posibility the general architecture is as follows:

.. image:: /_static/images/watchdog.png

The watchdog aggregator lives in the FiFo network. The components reporting to it are configured with it's ip. The watchdog server can be configured to send data upstream and get access to the watchdog-ui to view status and alerts.

Installation
------------

Decide what host you want to run the local watchdog service on and install it from the fifo package repository:

.. code-block:: bash

   pkgin in fifo-watchdog



Service Configuration
---------------------


Internal
````````
You need to configure Sniffle, Snarl and Chunter (those are the currently supported services) to send data to your local watchdog server adding something like this:


.. code-block:: ini

   ## the log level of the watchdog log
   ##
   ## Default: fifo
   ##
   ## Acceptable values:
   ##   - text
   log.watchdog.cluster = fifo

   ## the log level of the watchdog log
   ##
   ## Default: snarl
   ##
   ## Acceptable values:
   ##   - text
   log.watchdog.service = chunter

   ## the log level of the watchdog log
   ##
   ## Default: error
   ##
   ## Acceptable values:
   ##   - one of: debug, info, warning, error
   log.watchdog.level = error

   ## The ip of the watchdog server
   ##
   ## Acceptable values:
   ##   - an IP/port pair, e.g. 127.0.0.1:10011
   log.watchdog.host.local = 10.20.30.40:4444

If you have a newconfig those keys already exist, if not you can simply add them.

``log.watchdog.host.local`` needs to point to the IP of the server the ``fifo-watchdog`` package was installed.

``log.watchdog.level`` should be set to error, highter log levels will be send to the local watchdog but the upstream system will reject them.

``log.watchdog.cluster`` should be a sensible name for your cluster helping you to identify it, a cluster in this sense is a fifo instance. This allows for example to have a production and a dev cluster.

``log.watchdog.service`` should be set to the name of the service, ``chunter`` for the chunter service, ``sniffle`` for sniffle hosts and ``snarl`` for snarl hosts, please be sensible here.

Upstream
````````
Sending errors and alerts upstream requires a API token, as of now the upstream watchdog service is still in alpha and is on a invite only basis. That said if you're interested please contact us on IRC, Twitter or the mailinglist and we see if we can get you an account.


To setup the the upstream system it is required to adjust (or uncomment) two settings in the ``watchdog.conf`` (from the ``fifo-watchdog`` package):

.. code-block:: ini

   # Upstream server
   ##
   ## Acceptable values:
   ##   - an IP/port pair, e.g. 127.0.0.1:10011
   upstream = watchdog-upstream.project-fifo.net:4445

   ##
   ## Acceptable values:
   ##   - text
   # internal
   authtoken = your-token-goes-here


``upstream`` is the upstream endpoint, usually this does not need to be changed just uncommented.

``authtoken`` this is the auth token you recived or got from the watchdog-ui.
