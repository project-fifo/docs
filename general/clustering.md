---
layout: default
category: general
section: general/clustering
title: Project-FiFo Clustering
---
# Clustering

## Application Types
FiFo is built in a way to maintain the highest possible uptime: this means all components fall into one of two categories:
### Stateless
Maintaining availability in stateless applications is pretty simple, it's 'the cloud way' you can fire up multiple instances and since they don't need to share state, a failing instance can simply be replaced by a new one or discarded altogether.

The following components are stateless:

- wiggle
- jingles (with nginx)

<br/>

To maintain availability you will have to spawn at least two of each and put them behind a load balancer, this is an exercise for now left up to the user.

### Cluster/Distribution
Some components can't be stateless since they need to maintain some kind of data. To achieve a high level of availability, these components run in a clustered/distributed mode. FiFo uses riak core as a distribution layer, this means that it follows an 'eventual consistent' model and utilises all nodes that are present in the cluster, and can survive single nodes failing as long as a minimum number of nodes is present.

The following nodes are running based on riak core:

* Sniffle
* Snarl
* Howl

Being based on riak core, it is recommended to run at least 5 instances of this service on different hardware machines for ideal running operation. This cluster is called a ring in riak terms (this has to do with the dynamo paper for those interested).

### Chunter
Chunter is a special case since it is directly tied to a hardware node so high availability does not make sense for this component.

## Changing the cluster
If the cluster is set up before the system goes into production there is not mucht o worry about, however sometimes it is required to extend or shrink the cluster during production. Generally there is nothing that prevent this but it requries an additional step.

To prevent interruptions it is nessessariy to disable the `mdns` on the nodes to be added or removed, otherwise other services will see the node to be added or removed as a valid part of the cluster and try to access it. To disable `mdns` the configuration option `mdns.server = disabled` needs to be added to the configuration file of [Sniffle](/sniffle/configuration.html#mdns), [Snarl](/snarl/configuration.html#mdns) or [Howl](/howl/configuration.html#mdns) (depending on what service is added or removed), this should be done before the services is first started or removed / leaves.

For the following section the existing server will be named `sniffle@10.0.0.1` and the new server will be named `sniffle@10.0.0.2`, if multiple servers exist it does not matter which one is picked. Also keep in mind that for working with a howl or snarl cluster the section before the `@` needs to be replaced with `howl` and `snarl` respectively.

### New Cluster Node Preparation
Prepare a new FiFo node the same way the first node was created following the the [Installation](/general/installation.html) section. However, note that as the first Fifo node or existing FiFo cluster will likely already contain an admin account and that the last step is not needed. If additional admin accounts are created it will not fowl the join operation and clean up of redundancies can be performed within the UI or using the fifoadm command.

### Adding a server
To add a server the following steps need to be taken, exact descriptions of each commands can be found in the administration guide for [Sniffle](/sniffle/administration.html#cluster), [Snarl](/snarl/administration.html#cluster) and [Howl](/howl/administration.html#cluster).

- on the **existing server** check the server status with [ring-status](/sniffle/administration.html#ring-status) and [member-status](/sniffle/administration.html#member-status) commands.
- on the **new server** join the **existing server** with the [join](/sniffle/administration.html#join) command.
- on the **existing server** re check the server status with [ring-status](/sniffle/administration.html#ring-status) and [member-status](/sniffle/administration.html#member-status) commands.

### Removing a server
Removing a server is as simple as adding one, but to ensure it is not picked up again after it is removed the line `mdns.server = disabled` should be added to the configuration file of the server to be removed.

- on the **existing server** check the server status with [ring-status](/sniffle/administration.html#ring-status) and [member-status](/sniffle/administration.html#member-status) commands.
- on the **server to be removed** run the [leave](/sniffle/administration.html#leave) command.
- on the **existing server** re check the server status with [ring-status](/sniffle/administration.html#ring-status) and [member-status](/sniffle/administration.html#member-status) commands.
- once the server is removed it is restarted and can be shut down using the `svcadm disable sniffle` command.
