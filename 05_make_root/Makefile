KERNEL_MAJOR_VERSION       ?= 6
KERNEL_VERSION             ?= 6.1.47
BUSYBOX_VERSION            ?= 1.36.1
TOYBOX_VERSION             ?= 0.8.10
ARCH                       ?= x86_64
BUILD_DIR                  := build_dir
SUBDIRS                    := $(wildcard src/*/.)

.PHONY: help bootstrap clean download extract build package run  $(SUBDIRS)

help:                            ## printing out the help
	echo $(SUBDIRS)
	@echo
	@echo micropython-fun Makefile
	@echo
	@echo --- TARGETS ---
	@grep -F -h "##" $(MAKEFILE_LIST) | grep -F -v fgrep | sed -e 's/\\$$//' | sed -e 's/##//'

bootstrap:                       ## bootstrapping the virtualenv
	echo "currently nothing"

clean: $(SUBDIRS)                ## clean up everything
	rm -rf $(BUILD_DIR)

build: $(SUBDIRS)                ## build everything

package: $(SUBDIRS) $(BUILD_DIR)    ## package everything
	cp src/linux/$(BUILD_DIR)/linux-$(KERNEL_VERSION)/arch/x86/boot/bzImage $(BUILD_DIR)/linux-kernel
	rsync -av src/toybox/build_dir/toybox-$(TOYBOX_VERSION)/root/host/fs $(BUILD_DIR)/
	cd $(BUILD_DIR)/fs && find . | cpio -o -H newc | gzip > ../initramfs.cpio.gz

$(BUILD_DIR):
	mkdir -p $@

$(SUBDIRS):
	$(MAKE) -C $@ $(MAKECMDGOALS)

run:                             ## run everything
	qemu-system-x86_64 -m 256 -nographic -no-reboot -kernel $(BUILD_DIR)/linux-kernel -initrd $(BUILD_DIR)/initramfs.cpio.gz -append "panic=1 HOST=$(ARCH) console=ttyS0"
