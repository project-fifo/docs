.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

*****************
Stacks & Clusters
*****************

With the help of **Stacks** and **Clusters** it is possible to 'define' the design of applications deployed within the *FiFo* Cloud. This helps the system make intelligent decisions regarding VM placement.

.. image:: /_static/images/jingles/groupings01.png

The above view shows an overview of the Stacks and Clusters known to FiFo. They can be deleted with the little |trash| button at the end of each row. New Stacks or Clusters can be added using the **New Cluster** or **New Stack** button.

.. |trash| image:: /_static/images/jingles/groupings-destroy.png

Clusters
########

A **Cluster** is a collection of VMs. A Cluster guarantees that the VMs do not share the same physical hardware - thus increasing fault tolerance throughout the cluster. A **Cluster** can only be placed / assigned to a VM in the process of creatin that machine. After VM creation the **Cluster** can no longer guarantee placement. 

A VM can be removed from a **Cluster** at any point in time. Viewing a **Cluster** will show all the assigned VMs and **Stacks** it is a part of.

Stacks
######

Unlike a **Cluster** a **Stack** has **no** guarantees but offers a 'best effort' in placing VMs. A **Stack* is Built out of multiple *Clusters**. *FiFo* will try to place new VMs within a **Stack** (or therefore a **Cluster** within the **Stack**) and position it as close as possible to fellow VMs within the same **Stack**.

.. note::
	Only **Clusters** can belong to a **Stack**, VMs can't directly be placed in a **Stack**. However it is fully possible to make a **Cluster** with a single VM in it. This will also leave room for you to extend the future functionality of the **Cluster** when more VMs are thrown into the mix.

.. note::
	Since **Clusters** do not have guarantees, you may add or remove them at any time from a **Stack/s**.
