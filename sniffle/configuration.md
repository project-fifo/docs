---
layout: default
title: Sniffle installation
---
# Configuration file
Sniffle's configuration file is located in `/opt/local/sniffle/etc/sniffle.conf` it is automatically generated on the first install and not overwritten on updates. Non the less the newst version of the file is always located in `/opt/local/sniffle/etc/sniffle.conf.example`.

The configuration file is documented inline but we'll go over go over some more interesting settings here.

## Active Anti Entropy (AAE)

AAE is riak's mechanism of background syncronisation of systems to ensure a higher data consistency, it was ported to FiFo to offer incrased stability in multi node setups. AAE increases the required resources conciderably and does not have much use with a single system so it is **disabled by default**.

Having more then one system it strongly recommanded to enable AAE! It is possible to selectively enable and disable for different subsystems, generally it is OK keep it disabled for `images` since data does not chagne, all other systems should be switched on.

```
## enable active anti-entropy subsystem
anti_entropy = on
dataset.aae = on
hypervisor.aae = on
vm.aae = on
iprange.aae = on
network.aae = on
img.aae = off
dtrace.aae = on
package.aae = on
```

## Database

FiFo uses `leveldb` as it's backend databas, leveldb has many different tuneables some of the more important ones are.

### `ring_size`

The number of VNodes used by fifo, this setting can only be changed before the system is booted the first time so choose it carefully, it defaults to `8` but with more then one systems it is very sensible to increase this number higher, generally `~10` vnodes per physical node are a good rule of thumb. The `ring_size` needs to be a a power of two (`2`, `4`, `8` ... `64` ...).

### `leveldb.mmap_size`

The chunk size of each mmaped file, this has a huge impact of the memory requirements. Since fifo does not store lots of data a setting of `1MB` is a valid value for small and medium installations, the settings can be increased as long as it is ensured that enough memory is present.
