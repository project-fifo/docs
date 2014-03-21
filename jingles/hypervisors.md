---
layout: default
category: jingles
section: jingles/hypervisors
title: Jingles Hypervisors
---
# Jingles Hypervisors

## List view

![](/assets/img/jingles/hypervisors01.jpg)

The hypervisors section shows a list of all compute nodes / servers under FiFo's management.

- **Name** Clicking on the name of the hypervisor will take you to the hypervisor details page.
- **IPv4** The ip address of the hypervisors admin interface.
- **Booted** How long ago the hypervisor was booted.
- **Agent version** The version of the "chunter" service agent running on the hypervisor.
- **Ram** The total amount of ram installed in the compute node.
- **Free ram** The amount of free unallocated ram on the compute node.
- **Networks** The available networks configured on that compute node.
- **Capabilities** Shows if the node is capable of zone and KVM virtualization.

## Detauls
![](/assets/img/jingles/hypervisors02.jpg)

The hypervisor details page shows information and contains sections related to an individual hypervisor.

1. The name of the hypervisor / compute node. If hostname is not set the mac address of the admin interface will be used as the name.
The individual tabs relating to the hypervisor. Explained in more detail below.
2. The details tab contains a plethora of information relating to the hypervisor / compute node
  - **Information** Includes SmartOS version of node and the "zpool" type.
  - **CPU / mainboard** Includes cpu, motherboard and manufacturer information.
  - **Memory** Memory and L1 and L2 cache / arc statistics.
  - **Storage** pool sizes, used and free space as well as the "Health Status" of your pools.
  - **Networks** ip, nic name, driver and mac address details.
3. Clicking on the "Disks" hyperlink will expand the section and show you individual disk drive ID's and sizes.
    ![](/assets/img/jingles/hypervisors-disks.png)

### Performance
![](/assets/img/jingles/hypervisors03.jpg)

The hypervisor performance tab currently only contains a CPU load graph, but this will be expanded upon in future versions to include other metrics such as ram, disk, network utilisation.

### Characteristics
![](/assets/img/jingles/hypervisors04.jpg)

The characteristic "metadata" is a powerful addition to FiFo when used in conjunction with package rules and the inherent intelligence of FiFo's auto provisioning algorithm. For example you could create a package X that says when provisioning a vm with this package only provision on:

Compute nodes that `deployable` = `yes` with a `rack` = `2` and has a `dual-power` = `yes`
This is a awesome capability to ensure that if a certain package is used it will only deploy on specific nodes that have certain metadata attributes.

Some example hypothetical scenarios where this could be useful are:

 - Ensuring a certain user or group's vm's only gets deployed on a certain grade of hardware.
 - Ensuring that specific user or groups vm's do not get deployed on shared compute nodes e.g. private non shared physical hardware.
 - Ensuring that very big vm's with a big ram requirement only get deployed on nodes that have more than 256GB of ram and at least cpu's with 8 cores.

...and more... your imagination is the only limit.

To create or work with characteristics:

1. Click the <kbd>+</kbd> add button. Enter a key name for the new characteristic in the popup dialogue then click ok.
2. Enter some metadata for your specific key.
3. if you would like to delete a key and associated metadata, simply click the <kbd>-</kbd> delete button.
