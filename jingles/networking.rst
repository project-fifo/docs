.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

**********
Networking
**********

To get to *Fifo's* network management section: 

1. Click **More** on the home screen and than select **Configuration**.

2. On the next page select **Networking**

.. image:: /_static/images/jingles/networks_getto.png

____

Networks
########

List
****

Lists the Netowrks known to *FiFo*.

.. image:: /_static/images/jingles/networks01.png


1. By clicking the |new network| button you can create a new network.
2. By clicking the |delete network| button next to the network you can delete an existing network.

.. |new network| image:: /_static/images/jingles/networks-new.png
.. |delete network| image:: /_static/images/jingles/networks-delete.png

Create
******

Creating a new Network is simple. There are not many changes to be made:

.. image:: /_static/images/jingles/networks02.png

1. Enter the name of the network.
2. To create a new network click the |create button| button.

.. |create button| image:: /_static/images/jingles/create.png

Details
*******

.. image:: /_static/images/jingles/networks03.png

1. To add a new IPrange to the network select one from the selection panel: |selection panel|.
2. By clicking *delete* next to the IPrange you can delete that IPrange from the network.
3. By clicking the |trash icon| button you can delete the entire network.

.. |selection panel| image:: /_static/images/jingles/iprange-selection.png
.. |trash icon| image:: /_static/images/jingles/users-delete.png

____

IP-Ranges
#########

List
****

*FiFo* manages all IP provisioning. It stores a pool of IP addresses for each network you create. Upon machine creation *FiFo* assigns IP addresses and returns the IP addresses to the pool when a machine is destroyed. The address assignment works in conjunction with the security features built into *SmartOS* to ensure that only the IP address handed out can be used on the specific network you deploy on. This ensures a good neighbour policy; it prevents malicious users of machines from spoofing IP addresses and interfering with the traffic of other machines.

.. image:: /_static/images/jingles/ipranges01.png

The default network list view shows the networks that are currently configured and columns with information specific to each network:

- **Name**: You can chose any name you like.
- **Tag**: The network **Tag** must be a real network tag that aexists on at least one hypervosir/compute node.
- **Network**: The network address of your network.
- **Netmask**: The subnet mask of your network.
- **Gateway**: The network gateway.
- **First**: The first provision-able IP address in the range.
- **Last**: The last provision-able IP address in the range.
- **Next**: The next IP address that will be handed out.
- **VLAN**: The network VLAN ID (optional).
- **Returned**: The number of addresses that have been returned to the pool.
- **Action**: Delete the network by clicking on the **X** delete icon.

Create
******

To create a new network click on the |add network| button: 

.. |add network| image:: /_static/images/jingles/ipranges-add.png

.. image:: /_static/images/jingles/ipranges02.png

Populate the fields with network information relevant to the network you want to setup. The system will only allow you to select a valid network **Tag** that does in fact exist on one or more hypervisor/compute nodes.

Enter the **first** and **last** IP address in the range that you want *FiFo* to allocate.

When all required fields are filled in, the |create| button will appear.

Click on the |create| button to save your new network: 

.. |create| image:: /_static/images/jingles/create.png
