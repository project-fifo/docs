.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

********
Topology
********

**Topology** is where to tell *FiFo* the layout of your network and datacenter. This allows *FiFo* to make better decisions when placing your VMs. It does this by calculating the distances between hypervisors and specific VMs.

The **Topology** is expressed by giving each hypervisor a path. The "where" the first element in a path is the highest in the hierarchy and the lowest is the last. A top most element for each part is assumed but not shown since it can neither be modified nor deleted. This ensures all paths' are connected. The last element of a path should be the hypervisor itself which is also the default total path.

Paths can be changed by clicking on the hypervisor in the **Topology** view. New elements are added to the front of the path. The view is automatically changed to display the cloud layout in a tree like structure.

.. image:: /_static/images/jingles/topology01.png
