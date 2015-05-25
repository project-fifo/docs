.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*****
zfifo
*****

``zfifo`` is a command line tool that can be installed within a zone managed by fifo and can be used to control certain aspects from within the zone. This facilitates interaction with fifo without the need to authenticate.

The "zfifo" commands are limited to interacting with the zone itself and the related stack and/or cluster.

For further details on usage, please see ``man zfifo`` once the package has been installed.

Functionality
`````````````

* **snapsnots** : list and create
* **backups** : list and create
* **metadata** : get and set
* **cluster config** : get and set
* **stack config** : get and set

Installation
````````````

The installation is rather simple. The ``zfifo`` package is available in the fifo package repository and can be installed via ``pkgin install zfifo`` or:

    pkg_add http://release.project-fifo.net/pkg/rel/zfifo

(same goes for dev)



