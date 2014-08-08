---
layout: default
category: jingles
section: jingles/groupings
title: Jingles Stacks & Clusters
---
# Stacks & Clusters<a id="list"></a>

With the help of Stacks and Clusters it is possible to 'define' the design of applications deployed within the FiFo Cloud. This helps the system make intelligent decisions regarding VM placement.

![List](/assets/img/jingles/groupings01.png)

The above view shows an overview of the Stacks and Clusters known to FiFo. They can be deleted with the little `trash` button at the end of each row. New Stacks or Clusters can be added using the `New Cluster` or `New Stack` button.

## Clusters<a id="clusters"></a>
A **Cluster** is a collection of VM's. A Cluster guarantees that the VM's do not share the same physical hardware - thus increasing fault tolerance throughout the cluster. A Cluster can only be placed / assigned to a VM at create time. After VM creation the Cluster no longer can guarantee placement. 

A VM can be removed from a cluster at any point in time. Viewing a cluster will show all the assigned VM's and **Stacks** it  is part of.

## Stacks<a id="stacks"></a>
Unlike a **Cluster** a **Stack** has **no** guarantees but offers a 'best effort' in placing VM's. A stack is Built out of multiple clusters. FiFo will try to place new VM's within a stack (or therefore a cluster within the stack) and position it as close as possible to fellow VM's within the same stack.

**Please note:** only clusters can belong to a stack, VM's can't directly be placed in a stack. However it is fully possible to make a cluster with a single VM in it. This will also leave room for you to extend the future functionality of the cluster when more VM's are thrown into the mix.

Since clusters do not have guarantees, you may add or remove them at any time from a stack/s.
