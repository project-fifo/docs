---
layout: default
category: jingles
section: jingles/topology
title: Jingles Topology
---
# Topology

Topology is where tell FiFo the layout of your network and datacenter. This allows FiFo to make better decisions when placing your VMs. It does this by calculating the distances between hypervisors and specific VMs.

The topology is expressed by giving each hypervisor a path. The "where" the first element in a path is the highest in the hierarchy and the lowest is the last. A top most element for each part is assumed but not shown since it can neither be modified nor deleted, this ensures all path' are connected. The last element of a past should be the hypervisor itself which also is the default total past.

Paths can be changed by clicking on the hypervisor in the topology view, new elements added to the front of the path, the view is automatically changed to display the cloud layout in a tree like structure.

![List](/assets/img/jingles/topology01.png)
