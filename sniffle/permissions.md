---
layout: default
category: sniffle
section: sniffle/permissions
title: Permissions
---
# Sniffle Permissions

## Datasets<a id="datasets"></a>
Dataset Permissions start with `orgs->` followed by the UUID of the dataset. In addition to user specific permissions the following `cloud` global permission exist.

### `cloud->datasets->list`
Allows to list datasets, the `get` permission on each dataset determins which users are seen during the list.

### `cloud->datasets->create`
Allows the to create new datasets.

### `datasets-><UUID>->get`
Allows to see the dataset/read it's data.

### `datasets-><UUID>->delete`
Allows to delete the dataset.

### `datasets-><UUID>->edit`
Allows to change the dataset.

## D-Trace<a id="dtrace"></a>
D-Trace Permissions start with `orgs->` followed by the UUID of the dtrace. In addition to user specific permissions the following `cloud` global permission exist.

### `cloud->dtraces->list`
Allows to list dtraces, the `get` permission on each dtrace determins which users are seen during the list.

### `cloud->dtraces->create`
Allows the to create new dtraces.

### `dtraces-><UUID>->get`
Allows to see the dtrace/read it's data.

### `dtraces-><UUID>->delete`
Allows to delete the dtrace.

### `dtraces-><UUID>->edit`
Allows to change the script.

### `dtraces-><UUID>->stream`
Allows to run this script.

## Hypervisors<a id="hypervisors"></a>
Hypervisor Permissions start with `orgs->` followed by the UUID of the hypervisor. In addition to user specific permissions the following `cloud` global permission exist.

### `cloud->hypervisors->list`
Allows to list hypervisors, the `get` permission on each hypervisor determins which users are seen during the list.

### `cloud->hypervisors->create`
Allows the to create new hypervisors.

### `hypervisors-><UUID>->get`
Allows to see the hypervisor/read it's data.

### `hypervisors-><UUID>->delete`
Allows to delete the hypervisor.

### `hypervisors-><UUID>->edit`
Allows to change the hypervisor.

### `hypervisors-><UUID>->create`
Allows to create VM's on this hypervisor.

## IP-Ranges<a id="ipranges"></a>
IP-Range Permissions start with `orgs->` followed by the UUID of the iprange. In addition to user specific permissions the following `cloud` global permission exist.

### `cloud->ipranges->list`
Allows to list ipranges, the `get` permission on each iprange determins which users are seen during the list.

### `cloud->ipranges->create`
Allows the to create new ipranges.

### `ipranges-><UUID>->get`
Allows to see the iprange/read it's data.

### `ipranges-><UUID>->delete`
Allows to delete the iprange.

### `ipranges-><UUID>->edit`
Allows to change the iprange.

## Networks<a id="networks"></a>
Network Permissions start with `orgs->` followed by the UUID of the network. In addition to user specific permissions the following `cloud` global permission exist.

### `cloud->networks->list`
Allows to list networks, the `get` permission on each network determins which users are seen during the list.

### `cloud->networks->create`
Allows the to create new networks.

### `networks-><UUID>->get`
Allows to see the network/read it's data.

### `networks-><UUID>->delete`
Allows to delete the network.

### `networks-><UUID>->edit`
Allows to change the network.

## Packages<a id="packages"></a>
Package Permissions start with `orgs->` followed by the UUID of the package. In addition to user specific permissions the following `cloud` global permission exist.

### `cloud->packages->list`
Allows to list packages, the `get` permission on each package determins which users are seen during the list.

### `cloud->packages->create`
Allows the to create new packages.

### `packages-><UUID>->get`
Allows to see the package/read it's data.

### `packages-><UUID>->delete`
Allows to delete the package.

### `packages-><UUID>->edit`
Allows to change the package.

## VMs<a id="vms"></a>
VMs Permissions start with `orgs->` followed by the UUID of the VM. In addition to user specific permissions the following `cloud` global permission exist.

### `cloud->packages->list`
Allows to list packages, the `get` permission on each package determins which vm are seen during the list.

### `cloud->packages->create`
Allows the to create new Vm.

### `vms-><UUID>->get`
Allows to see the VM/read it's data.

### `vms-><UUID>->delete`
Allows to delete the VM.

### `vms-><UUID>->edit`
Allows to change the VM.

### `vms-><UUID>->start`
Allows to start the VM.

### `vms-><UUID>->stop`
Allows to stop the VM

### `vms-><UUID>->reboot`
Allow to reboot the VM.

### `vms-><UUID>->console`
Allows acces to the VMs Console or VNC, also implies full access to all SSH users.

### `vms-><UUID>->ssh-><USER>`
Allows the the access to SSH into the given user (i.e. `<USER>` could be `root`)

### `vms-><UUID>->snapshot`
Allows to take a snapshot.

### `vms-><UUID>->rollback`
Allows to rollback a snapshot.

### `vms-><UUID>->snapshot_delete`
Allows to delete a snapshot

### `vms-><UUID>->backup`
Allows to create a backup.

### `vms-><UUID>->rollback`
Allows to rollback a backup.

### `vms-><UUID>->backup_delete`
Allows to delete a backup.
