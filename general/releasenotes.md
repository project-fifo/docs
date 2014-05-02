---
layout: default
category: general
section: general/releasenotes
title: Release Notes
---


## 0.4.4<a id="0.4.4"></a>

* A important update to the Active Active Entropy engine.
* Commands to perform database updates.
* Diagnostic commands (sniffle-diag, snarl-diag, ...).
* The commands sniffle, snarl, sniffle-admin, and snarl-admin are now put into /opt/local/sbin.
* A chineese translation of the UI.
* More improvements on the config files.
* Minor fixes and improvements.

## 0.4.3<a id="0.4.3"></a>

This is the so far biggest release of Project-FiFo, aside of the usual improvements and fixes we've added a good number of new and exciting features!

<p class="bs-callout bs-callout-warning">
With 0.4.3 we have made the decision to tune defaults towards a 1 node installation instead of a full-blown multi node setup.<br/>
The reasoning for this is simple, one node installations are usually done by people trying out the project for the first time, mostly without a firm gasp of what it is and what they are wanting to do with it. Those installations are usually the same across the board so having a default that 'fits all' is much easier. Also scaling up is easier then scaling down once the installation grows.<br/>
The consequence of this is if you are using FiFo in production it is more important then ever to plan ahead and understand the environment in which you are running it to pick some sane defaults of your use case, but given that usually production environments are much more diverse then test environments this step would have been highly recommended even before this decision.
</p>

### Configuration

#### New configuration files

Starting with this release we use [Cuttlefish](https://github.com/basho/cuttlefish) which means configurations files are now much easier to maintain and more ops friendly adopting a simple `key = value` syntax for settings instead of Erlang syntax that most people are not familiar with. This change however requires some additional work as described in the [upgrade instructions](/general/upgrade.html#0.4.3).

#### Global configuration

With some of the settings (like the s3 or yubikey settings) globally valid it makes sense to not require them to be configured for each node, so they are now globally configurable with `sniffle-admin config` and `snarl-admin config`, exact details are described in the [sniffle configuration](/sniffle/configuration.html#global) and [snarl configuration](/snarl/configuration.html#global) section.

#### Reserved memory

It is now possible to reserve memory on the hypervisor this makes managing things like ARC memory or just to guarantee some memory for the global zone. This can be done in the [chunter configuration file](/chunter/configuration.html#file).

#### Improved chunter installation

Upon the first install chunter tries to guess the IP configuration of the system to determine what interface to use to talk to the other services. This now checks for a `fifo0` interface it will favor over the `admin` interface when present allowing for easier setup in large environments.

### Snarl

#### Multi-Datacenter support

Starting with this release Snarl allows for synchronizing logins, roles, organizations and tokens over multiple data centers. This makes it possible to combine multiple data centers into one logical FiFo unit while still keeping the datacenter operation tasks separated to prevent dependencies and possible downtime. This requires [setting up synchronization endpoints](/snarl/configuration.html#multidc) between involved services.

#### Yubikey support

A cloud requires security, and while a normal username/password login is nice and good it sometimes isn't enough. Yubikeys are a very simple answer to multi factor authentication and supporting them is now a build in feature in Snarl. The configuration is rather easy and can be done via the new [global configuration](/snarl/configuration.html#yubikey).

### Sniffle

#### LeoFS (S3) integration

FiFo can now integrate with [LeoFS](http://leofs.org) (or probably any other S3 API) to separate storage from management. With S3 storage enabled datasets get stored in S3 instead of Sniffle directly improving scalability and performance. The needed configuration can be handled via the global [sniffle configuration](/sniffle/configuration.html#global) and is automatically synchronized between all hosts.

#### Backups

<p class="bs-callout bs-callout-danger">
Please be warned that backups are still a experimental feature and while they have not shown any failures during the time 0.4.3 was in development mode they should not be relied on as the only copy of sensitive data.
</p>

In addition to storing datasets the introduction of a external storage made it possible to introduce a entirely new feature, backups. Backups are the logical conclusion from the already existing snapshots for VM's just instead of being stored on the hypervisor with the VM they are relocated to a separate storage system.

The separation from the VM allows for a number of new features like reverting backups ups without loosing newer versions, recovering VM's after total hypervisor loss, and last but not least moving VM's from one hypervisor to another using the backup to restore them.

#### Datasets

There are new API calls for importing and exporting datasets, making it easy to push custom datasets to fifo or downloading datasets form FiFo to publish to places like [datasets.at](http://datasets.at). However client support for those commands is still pending.

#### VM Creation

The logic behind the creation process of VM's have been significantly altered and improved, the new logic prevents race conditions when creating multiple VM's in parallel, overloading of `vmadm` by queuing creates on a per hypervisor level and potential issues in the case of a network split. The new code has been tested with **1:300** ratio of hypervisors/creation-requests and performs safely under this conditions.


#### Services

FiFo now allows managing services (as in SMF services) over the FiFo API this means it's easy to monitor the state of a service or change it if needed. This is possible for both the Global Zone and SmartOS VMs.

#### Hypervisor updates

Sniffle now contains code to trigger chunter updates, this simplifies the management of large amounts of hypervisors since it requires just one command to update all of them instead of doing it on each one on it's own. However it still is possible to trigger single hypervisors either locally or from fifo directly.

The related commands in the FiFo Zone are: `fifoadm hypervisors update` and `fifoadm hypervisors update <hypervisor>`.

### General

#### Active Anti Entropy(AAE)

<p class="bs-callout bs-callout-warning">
AAE is disabled by default since it does not make sense to use it with less then two nodes.
</p>

We back-ported the code used in [riak for Active Anti Entropy](https://basho.com/tag/active-anti-entropy/)  to increase the consistency of data within FiFo, this minimizes the chance of less frequently accessed data.

#### Memory consumption

<p class="bs-callout bs-callout-warning">
The tuning done to reduce memory consumption reflects directly a reduced performance for larger installations, if you are running more then one node please be sure to have a look at configuration values as the number of vnodes or the mmap_size settings for sniffle, snarl and howl.
</p>

With the amount of services running inside of FiFo the amount of memory it required grows steadily, with the 0.4.3 release we've taking steps to significantly reduce the memory requirement to a bearable level. All the changes done are covered by configuration values and tuned for a 1 node installation to make starting off easy, it is worth investing some time considering changes.

### UI

#### Jingles

The entire user interface have been reworked, providing a lighter and more pleasant look, improving usability and give a better logical structure of the components.

#### Documentation

Along with the rework of the interface the entire documentation has been redone making it easier to access and more detailed on content.
