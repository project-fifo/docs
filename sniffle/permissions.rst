.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.


***********
Permissions
***********

Datasets
########

Dataset Permissions start with ``datasets->`` followed by the UUID of the dataset. In addition to user specific permissions the following ``cloud`` global permission exist.

``cloud->datasets->list``
    Allows to list datasets, the ``get`` permission on each dataset determines which users are seen during the list.

``cloud->datasets->create``
    Allows the to create new datasets.

``datasets-><UUID>->get``
    Allows to see the dataset/read it's data.

``datasets-><UUID>->delete``
    Allows to delete the dataset.

``datasets-><UUID>->edit``
    Allows to change the dataset.

D-Trace
#######

D-Trace Permissions start with ``dtraces->`` followed by the UUID of the dtrace. In addition to user specific permissions the following ``cloud`` global permissions exist.

``cloud->dtraces->list``
    Allows to list dtraces, the ``get`` permission on each dtrace determines which users are seen during the list.

``cloud->dtraces->create``
    Allows the to create new dtraces.

``dtraces-><UUID>->get``
    Allows to see the dtrace/read it's data.

``dtraces-><UUID>->delete``
    Allows to delete the dtrace.

``dtraces-><UUID>->edit``
    Allows to change the script.

``dtraces-><UUID>->stream``
    Allows to run this script.

Hypervisors
###########

Hypervisor Permissions start with ``hypervisors->`` followed by the UUID of the hypervisor. In addition to user specific permissions, the following ``cloud`` global permissions exist.

``cloud->hypervisors->list``
        Allows to list hypervisors, the ``get`` permission on each hypervisor determines which users are seen during the list.

``cloud->hypervisors->create``
    Allows the to create new hypervisors.

``hypervisors-><UUID>->get``
    Allows to see the hypervisor/read it's data.

``hypervisors-><UUID>->delete``
    Allows to delete the hypervisor.

``hypervisors-><UUID>->edit``
    Allows to change the hypervisor.

``hypervisors-><UUID>->create``
    Allows to create VM's on this hypervisor.

IP-Ranges
#########

IP-Range Permissions start with ``ipranges->`` followed by the UUID of the iprange. In addition to user specific permissions, the following ``cloud`` global permissions exist.

``cloud->ipranges->list``
    Allows to list ipranges, the ``get`` permission on each iprange determines which users are seen during the list.

``cloud->ipranges->create``
Allows the to create new ipranges.

``ipranges-><UUID>->get``
    Allows to see the iprange/read it's data.

``ipranges-><UUID>->delete``
    Allows to delete the iprange.

``ipranges-><UUID>->edit``
    Allows to change the iprange.

Networks
########

Network Permissions start with ``networks->`` followed by the UUID of the network. In addition to user specific permissions, the following ``cloud`` global permissions exist.

``cloud->networks->list``
    Allows to list networks, the ``get`` permission on each network determines which users are seen during the list.

``cloud->networks->create``
    Allows the to create new networks.

``networks-><UUID>->get``
    Allows to see the network/read it's data.

``networks-><UUID>->delete``
    Allows to delete the network.

``networks-><UUID>->edit``
    Allows to change the network.

Packages
########

Package Permissions start with ``packages->`` followed by the UUID of the package. In addition to user specific permissions, the following ``cloud`` global permissions exist.

``cloud->packages->list``
    Allows to list packages, the ``get`` permission on each package determines which users are seen during the list.

``cloud->packages->create``
Allows the to create new packages.

``packages-><UUID>->get``
    Allows to see the package/read it's data.

``packages-><UUID>->delete``
    Allows to delete the package.

``packages-><UUID>->edit``
    Allows to change the package.

VMs
###

VMs Permissions start with ``vms->`` followed by the UUID of the VM. In addition to user specific permissions, the following ``cloud`` global permissions exist.

``cloud->packages->list``
    Allows to list packages, the ``get`` permission on each package determines which vm are seen during the list.

``cloud->packages->create``
    Allows the to create new Vm.

``vms-><UUID>->get``
    Allows to see the VM/read it's data.

``vms-><UUID>->delete``
    Allows to delete the VM.

``vms-><UUID>->edit``
    Allows to change the VM.

``vms-><UUID>->start``
    Allows to start the VM.

``vms-><UUID>->stop``
    Allows to stop the VM

``vms-><UUID>->reboot``
    Allows to reboot the VM.

``vms-><UUID>->console``
    Allows access to the VMs Console or VNC, also implies full access to all SSH users.

``vms-><UUID>->ssh-><USER>``
    Allows the the access to SSH into the given user (i.e. ``<USER>`` could be ``root``)

``vms-><UUID>->snapshot``
    Allows to take a snapshot.

``vms-><UUID>->rollback``
    Allows to rollback a snapshot.

``vms-><UUID>->snapshot_delete``
    Allows to delete a snapshot

``vms-><UUID>->backup``
    Allows to create a backup.

``vms-><UUID>->rollback``
    Allows to rollback a backup.

``vms-><UUID>->backup_delete``
    Allows to delete a backup.
