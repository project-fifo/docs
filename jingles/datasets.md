---
layout: default
category: jingles
section: jingles/datasets
title: Jingles Datasets
---
# Jingles Datasets

## Installed Datasets

![](/assets/img/jingles/datasets01.jpg)

FiFo incorporates its own internal dataset repository. It uses this repository to deploy datasets to individual SmartOS compute nodes that are under FiFo's management. FiFo is designed with no single point of failure in mind. By default the dataset storage database breaks each dataset into redundant shards and distributes it across your FiFO cluster (if setup in a clustered configuration). This means that datasets will always be available for provisioning.

When a request to provision a new machine is received, FiFO consults its internal intelligent auto provisioning algorithm to make some decisions. It looks at the package requested for the machine and then determines which compute node would be most suitable to deploy on. Once it has decided on the compute node, FiFo looks at the selected node and determines if that dataset already exists on that node. If it exists it continues with machine creation. If it does not yet exist, FiFo deploys the dataset to that compute node from its internal dataset repository and once deployed will then continue with machine creation.

The installed datasets tab shows you which datasets exist in the internal repository. The tick icon signifies that it has been imported successfully. During importation a progress bar will show the % percentage complete in realtime and will be replaced with a tick once complete.

![](/assets/img/jingles/datasets02.jpg)

To delete a dataset and remove it from the repository click on the "x" delete icon next to the individual dataset.

## Remote Datasets

![](/assets/img/jingles/datasets03.jpg)

The remote datasets tab is where you choose which external datasets you would like to import into FiFo's internal dataset repository. By default FiFo is pre-configured to pull datasets from http://datasets.at the community dataset repository which contains all the official Joyent datasets as well as community contributed datasets.

1. To "import" a dataset simply click on the import button. The importing dataset progress bar will then be visible in the "dataset installed" tab.
2. Shows the external repository FiFO is currently associated with.

Should you wish to change the remote repository source this can be configured by editing the following configuration file.

### /opt/local/jingles/app/scripts/config.js

```
/*
 *      The URL of the remote dataset repository
 */
datasets: 'datasets.at',
```
