.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

********
Packages
********

1. `List`_
2. `Creation`_

	- `Package rules`_
	- `weight`_
	- `condition`_
	- `attribute`_
	- `usable attributes`_
3. `Details`_

List
####

.. image:: /_static/images/jingles/packages01.jpg

The packages list page gives you an overview of existing packages and is the place where you create additional packages.

- **Name** The name of the package.
- **Ram** How much Ram is allocated with the package.
- **Disk** How much disk is allocated with the package.
- **CPU cap** How much CPU is allocated with the package. This is a percentage, and 100% is equivalent to 1 cpu.
- **Actions** The delete icon <kbd>x</kbd> is used to delete a package.

Clicking on the package name / hyperlink will bring up the package details page.

.. image:: /_static/images/jingles/packages02.jpg

Creation
########

Clicking on the **Add Package** button will take you to the package creation page as shown below:

.. image:: /_static/images/jingles/packages03.jpg


1. Enter the name of the package.
2. Fill in cpu, ram and disk to be used for the package.
3. Create optional package rules if you require them. example:
    .. image:: /_static/images/jingles/packages04.jpg

    In the above example we are essentially saying that the **"uuid"** of the hypervisor must equal "**ee1c3236-a2b1-43fa-ac52-8d6d9c7a3dc3**". The result of choosing this package when creating a machine will ensure that the vm/machine only gets created on the hypervisor / compute node you specified.

4. Click on the **+** plus button to create additional package rules.

.. note::
	There are many additional keys and attributes you can use when creating package rules and they are outlined in more detail below.

		`Package rules`_ : `weight`_ `|` `condition`_ `|` `attribute`_ `|` `value`_ 


Package rules
*************

weight
******

* ``must`` - the hypervisor MUST satisfy this requirement or be discarded
* ``cant`` -  the hypervisor CAN'T satisfy this requirement or be discarded
* N (integer) - If this rule matches the weight of this hypervisor is adjusted with N (0 is the base value for all hypervisors)

condition
*********

* ``>=`` - The value of the hypervisor must be greater or equal then the comparison value
* ``>`` - The value of the hypervisor must be greater then the comparison value
* ``=<`` - The value of the hypervisor must be lower or equal then the comparison value
* ``<`` - The value of the hypervisor must be lower then the comparison value
* ``=:=`` - The value of the hypervisor must be equal to the comparison value
* ``=/=`` - The value of the hypervisor can't be equal to the comparison value
* ``subset`` - The the comparison value must be a subset of the hypervisor value
* ``superset`` - The the comparison value must be a superset of the hypervisor value
* ``disjoint`` - The the comparison value must be a disjoint with the hypervisor value
* ``element`` - The the comparison value must be an element of the hypervisor value

attribute
*********

* Any JSON path as string - This describes how the hypervisor value will be found (i.e. "resources.free-memory")

value
*****
* integer, string, array - the value that will be tested against.


usable attributes
******************

- ``resources.free-memory`` - amount of free memory available
- ``resources.total-memory`` - total memory on the node
- ``resources.provisioned-memory`` - node memory allocated for both running and stopped vm's
- ``resources.free`` - free disk storage available on the pool in MB e.g. `11871`
- ``resources.size`` - the size of the zpool in MB e.g. `20352`
- ``resources.used`` - used disk space on the pool in MB e.g. `8481`
- ``pools.*.free`` - free size in pool `zones` e.g. `8481`
- ``health`` - the status of the zpool e.g. `"ONLINE"`
- ``networks`` - name of the network tag e.g. `"admin"`
- ``uuid`` - uuid of the hypervisor
- ``alias`` - hypervisor name, e.g. `"00-0c-29-18-ec-10"` or `"compute-node-2"`
- ``virtualisation`` - node virtualisation type capability e.g. `"zone"` or `"kvm"`

Details
#######

.. attention::

	content is missing
