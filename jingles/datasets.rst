.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

********
Datasets
********

Installed Datasets
##################

.. image:: /_static/images/jingles/datasets01.jpg


*FiFo* incorporates its own internal dataset repository. It uses this repository to deploy datasets to individual *SmartOS* compute nodes that are under *FiFo's* management. *FiFo* is designed with no single point of failure in mind. By default the dataset storage database breaks each dataset into redundant shards and distributes it across your *FiFo* cluster (if setup in a clustered configuration). This means that datasets will always be available for provisioning.

When a request to provision a new machine is received *FiFo* consults its internal intelligent auto provisioning algorithm to make some decisions. It looks at the package requested for the machine and then determines which compute node would be most suitable to deploy on. Once it has decided on the compute node *FiFo* looks at the selected node and determines if that dataset already exists on that node. If it exists it continues with machine creation. If it does not yet exist *FiFo* deploys the dataset to that compute node from its internal dataset repository and once deployed it will than continue with machine creation.

The installed datasets tab shows you which datasets exist in the internal repository. The tick icon indicates that it has been imported successfully. During importation a progress bar will show the % percentage completed in real time and will be replaced with a tick once complete.

.. image:: /_static/images/jingles/datasets02.jpg

To delete a dataset and remove it from the repository click on the "**X**" delete icon next to the individual dataset.


Remote Datasets
###############

.. image:: /_static/images/jingles/datasets03.jpg

The remote datasets tab is where you choose which external datasets you would like to import into *FiFo's* internal dataset repository. By default *FiFo* is pre-configured to pull datasets from http://datasets.at the community dataset repository which contains all the official *Joyent* datasets as well as community contributed datasets.

	1. To "import" a dataset simply click on the import button. The importing dataset progress bar will then be visible in the "dataset installed" tab.
	2. Shows the external repository *FiFo* is currently associated with.

Should you wish to change the remote repository source this can be configured by editing the `configuration file <configuration.html>`_.


Details
#######

.. image:: /_static/images/jingles/datasets04.png

The dataset details allow edit some details of a dataset after it was imported.

	1. The currently attached NIC's can be deleted one by one.
	2. Allows adding a new required NIC.
	3. Saves the changes.


