KERNEL_MAJOR_VERSION       ?= 6
KERNEL_VERSION             ?= 6.1.47
BUSYBOX_VERSION            ?= 1.36.1
TOYBOX_VERSION             ?= 0.8.9
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

build: $(SUBDIRS)                ## build everything

package: $(SUBDIRS) $(BUILD_DIR)    ## package everything
	cp src/initrd/init $(BUILD_DIR)
	rsync -av src/toybox/build_dir/toybox-$(TOYBOX_VERSION)/install/* build_dir/
	cd $(BUILD_DIR) && find . | cpio -o -H newc | gzip > root.cpio.gz

$(BUILD_DIR):
	echo mkdir -p $@

$(SUBDIRS):
	$(MAKE) -C $@ $(MAKECMDGOALS)

run:                             ## run everything
	qemu-system-x86_64 -nographic -no-reboot -kernel src/linux/$(BUILD_DIR)/linux-$(KERNEL_VERSION)/arch/x86/boot/bzImage -initrd $(BUILD_DIR)/root.cpio.gz -append "panic=1 console=ttyS0"
