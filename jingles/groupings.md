---
layout: default
category: jingles
section: jingles/groupings
title: Jingles Stacks & Clusters
---
# Stacks & Clusters<a id="list"></a>

With the help of Stacks and Clusters it is possible to 'explain' the design of a application deployed on the FiFo Cloud to the system and help making decisions regarding VM placement.

![List](/assets/img/jingles/groupings01.png)

In this view one can get a overview of the Stacks and Clusters known to FiFo. They can be deleted with the little `x` button at the end of each row and new Stacks or Clusters added via the `New Cluster` or `New Stack` button.

## Clusters<a id="clusters"></a>
A **Cluster** is a collection of VM's, where the Cluster guarantees that the VM's do not share the same physical hardware - this increasing fault tollerance throught the cluster. A Cluster can only be assigned to a VM at create time, since after the fact the guarantee priovided by Clusters could not be provided.

![List](/assets/img/jingles/groupings02.png)

However it is possible to remove VM's from a cluster at any point in time. Viewing a cluster shows all the assigned VM's and the **Stacks** it is part of.

## Stacks<a id="stacks"></a>
Unlike a **Cluster** a **Stack** has no guarantees but offers a 'best effort' in placing VM's. A stack is Bouild out of multiple clusters an FiFo will try to place new VM's within a stack (or therefore a cluster within the stack) as close as possible to other VM's in the stack.

Please not that only clusters can belong to a stack, VM's can't directly be placed here. However it is fully possible to make a cluster with a single VM in it - this also allows to later extend the functionality of this cluster into more VM's.

![List](/assets/img/jingles/groupings03.png)

Note that, since no guarantees are given, Clusters can be added or removed from stacks at any time.
