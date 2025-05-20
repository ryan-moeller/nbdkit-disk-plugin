## FreeBSD disk device plugin for nbdkit NBD server

[nbdkit](https://gitlab.com/nbdkit/nbdkit) does not support serving FreeBSD disk
devices such as ATA disks, SCSI disks, or ZFS volumes.  This plugin implements
the basic necessities for serving the devices.

For a more feature-rich FreeBSD disk devices plugin with a runtime-reloadable
libucl configuration file, see
[nbdkit-disks-plugin](https://github.com/ryan-moeller/nbdkit-disks-plugin).

## Prerequisites

The only dependency is nbdkit, which can be installed by `pkg` or from ports.

The port/package is quite outdated, and more functionality can be enabled by
building and installing nbdkit from source before building the plugin.  The
version in ports does not support reporting block size properties of the device.

The example below uses git to clone the sources from GitHub, but one could
simply download the sources as a ZIP from GitHub using fetch.

## Building

Clone, build, and install the plugin:

```
$ git clone https://github.com/ryan-moeller/nbdkit-disk-plugin.git
$ cd nbdkit-disk-plugin
$ make
# make install # (optional) avoid needing to specify full path to shared library
```

## Usage

Serve a 40GB ZFS volume named `storage/nbdvol` without running `make install`:

```
# zfs create -V 40G storage/nbdvol
# nbdkit ./nbdkit-disk-plugin.so /dev/zvol/storage/nbdvol
```

Serve `/dev/md0` after running `make install`:

```
# nbdkit disk /dev/md0
```
