---
layout: default
category: jingles
section: jingles/topology
title: Jingles Topology
---
# Topology

Topology allows you explain the layout of your network and datacenter to FiFo, this allows FiFo to make bett decisions when placing your VMs by being able to calculate the distance between hypervisors and by that Mv's.

The topology is expressed by giving each hypervisor a path, the where the first element in a path is the highest in the herachy and the lowest is the last. A top most element for each part is assumed but not shown since it can neither be modifed nor deleted, this ensures all path' are conncted. The last element of a past should be the hypervisor itself which also is the default total past.

Pathes can be changed by clicking on the hypervisor in the topoligy view, new elements added to the front of the path, the view is automatically cahnged to display the cloud layout in a tree like strucutre.

![List](/assets/img/jingles/topology01.png)
