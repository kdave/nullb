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
