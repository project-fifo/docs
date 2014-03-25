---
layout: default
category: jingles
section: jingles/networking
title: Jingles Networking
---
# Jingles Networking


## Networks<a id="networks"></a>

### List<a id="networks-list"></a>
![](/assets/img/jingles/networks01.png)

Lists the Netowrks known to FiFo.

1. Allows deleting a Network.
2. Adds a new Network.

### Create<a id="networks-new"></a>
![](/assets/img/jingles/networks02.png)

Creating a new Network is simple there are not many changes to be made.

1. The name of the network.
2. Click to create a network.

### Details<a id="networks-details"></a>
![](/assets/img/jingles/networks03.png)

1. Select a iprange to add to the network.
2. Delete a iprange form a network.
3. Deletes the entire iprange.

## IP-Ranges<a id="ipranges"></a>

### List<a id="ipranges-list"></a>

![](/assets/img/jingles/ipranges01.png)

FiFo manages all IP provisioning. It stores a pool of IP addresses for each network you create. Upon machine creation FiFo assigns IP addresses and returns the IP addresses to the pool when a machine is destroyed. The address assignment works in conjunction with the security features built into SmartOS to ensure only the IP address handed out can be used on the specific network you deploy on. This ensures a good neighbour policy; it prevents malicious users of machines from spoofing IP addresses and interfering with other machines traffic.

The default network list view shows the networks that are currently configured and columns with information specific to each network:

- **Name** The name of the network, this can be anything you like.
- **Tag** The network "Tag" must be a real network tag that actually exists.
- **Network** The network address of your network.
- **Netmask** The subnet mask of your network.
- **Gateway** The network gateway.
- **First** The first provision-able IP address in the range.
- **Last** The last provision-able IP address in the range.
- **Next** The next IP address that will be handed out.
- **VLAN** The network VLAN ID (optional).
- **Returned** The number of addresses that have been returned to the pool.
- **Action** Delete the network by clicking on the <kbd>x</kbd> delete icon.

### Create<a id="ipranges-new"></a>

To create a new network click on the "add network" button: ![](/assets/img/jingles/ipranges-add.png)

![](/assets/img/jingles/ipranges02.png)

Populate the fields with network information relevant to the network you want to setup. The system will only allow you to select a valid network "Tag" that does in fact exist on one or more hypervisor /compute nodes.

Enter the "first" and "last" IP address in the range that you want FiFo to allocate.

When all required fields are filled in, the "Create" button will appear.

Click on the "Create" button to save your new network: ![](/assets/img/jingles/ipranges-create.png)
