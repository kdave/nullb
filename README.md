# nullb

Simple null block device manager script, useful for zoned device testing. The
devices are managed by configfs files, this script wraps the basic operations
for convenience.  The null block device is usually memory-backed so the
capacity is limited but it should be sufficient to create 1GiB or 2GiB devices.
The benefit is flexible zone size that could be otherwise unavailable with
other types of device.

Requirements:
- nullb\_blk loadable kernel module
- configfs either as loadable module or built-in

The script follows common command line interface when there are multiple
subcommands provided by one script. Currently there is:

* **nullb setup** - load required modules and mount configfs
* **nullb create [-s sizeMB] [-z zonesizeMB]** - create a new device with given sizes, allocating the first free indes, device is /dev/nullb$index
* **nullb ls**
* **nullb list** - show table of created null\_blk devices, size and zone sizes
* **nullb rm NAME**
* **nullb delete NAME** - delete existing null\_blk device by name, must match the device node name like *nullb0*

Defaults:
- device size: 2GiB
- zone size: 256MiB

# Quick start

```
# ./nullb setup
INFO: setup module and mounts
```

If that succeeds, the module has been loaded and the configfs is available.

```
# ./nullb create
INFO: Create nullb0
INFO: name=nullb0
INFO: size=2048M zone_size=256M
/dev/nullb0
```

The INFO messages are for clarity when run interactively, for scripting the
last line contains the name of the device node that's been created.

```
# ./nullb ls
INFO: device nodes:
/dev/nullb0
INFO: created devices:
No  Name      Device                   Size    Zone size  On
 0  nullb0    /dev/nullb0             2048M         256M   1
```

The null block device nodes and the actual name in sysfs can be different so
both are printed, though this script keeps both names respecting the index and
name matches the device node filename.

```
# ./nullb rm nullb0
INFO: check mounts
INFO: removing nullb0
```

Mount checks just lists all mounts using the device but won't stop the removal.
It should fail if the device is in use, otherwise the device and its resources
are released after the command ends.
