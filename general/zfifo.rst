.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*****
zfifo
*****

``zfifo`` is a command line tool that can be installed in a zone managed by fifo to control certain aspects from the zones. This allows to interact with fifo without the need to authenticate.

The commands that are available are liminted to interacting with the zones itself and the related stack and or cluster.

For details on the usage plase see ``man zfifo`` after installing the package.

Installation
````````````

The installation is rather simple the ``zfifo`` package is available in the fifo package reposotory and can be installed via ``pkgin install zfifo`` or:

    pkg_add http://release.project-fifo.net/pkg/rel/zfifo

(same goes for dev)
