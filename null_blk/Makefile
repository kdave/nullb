# SPDX-License-Identifier: GPL-2.0

OBJECT_FILES_NON_STANDARD := y
KDIR ?= /lib/modules/`uname -r`/build

# needed for trace events
ccflags-y			+= -I$(src)

obj-m				+= null_blk.o
null_blk-objs			:= main.o
ifeq ($(CONFIG_BLK_DEV_ZONED), y)
null_blk-$(CONFIG_TRACING) 	+= trace.o
endif
null_blk-$(CONFIG_BLK_DEV_ZONED) += zoned.o

default:
	$(MAKE) -C $(KDIR) M=$$PWD

clean:
	$(MAKE) -C $(KDIR) M=$$PWD clean

modules_install:
	$(MAKE) -C $(KDIR) M=$$PWD modules_install
