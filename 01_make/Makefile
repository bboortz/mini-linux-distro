KERNEL_MAJOR_VERSION       ?= 6
KERNEL_VERSION             ?= 6.1.47
BUILD_DIR                  := build_dir
SUBDIRS                    := $(wildcard */.)

.PHONY: help bootstrap clean download extract build package run  $(SUBDIRS)

help:                            ## printing out the help
	@echo
	@echo micropython-fun Makefile
	@echo
	@echo --- TARGETS ---
	@grep -F -h "##" $(MAKEFILE_LIST) | grep -F -v fgrep | sed -e 's/\\$$//' | sed -e 's/##//'

bootstrap:                       ## bootstrapping the virtualenv
	echo "currently nothing"


clean: $(SUBDIRS)                ## clean up everything
build: $(SUBDIRS)                ## build everything
package: $(SUBDIRS)              ## package everything

$(SUBDIRS):
	$(MAKE) -C $@ $(MAKECMDGOALS)

run:                             ## run everything
	qemu-system-x86_64 -nographic -no-reboot -kernel linux/$(BUILD_DIR)/linux-$(KERNEL_VERSION)/arch/x86/boot/bzImage -initrd init/$(BUILD_DIR)/root.cpio.gz -append "panic=1 console=ttyS0"
