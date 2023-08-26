BUILD_DIR                  := build_dir
KERNEL_MAJOR_VERSION       ?= 6
KERNEL_VERSION             ?= 6.1.47
DOWNLOAD_FILE              := $(BUILD_DIR)/linux-$(KERNEL_VERSION).tar.xz
EXTRACT_DIR                := $(BUILD_DIR)/linux-$(KERNEL_VERSION)
NPROCS                     := $(shell grep -c ^processor /proc/cpuinfo)
NPROCS                     := $(shell echo "$(NPROCS)-2"|bc)
MAKEFLAGS                  += -j $(NPROCS)


help:                            ## printing out the help
	@echo
	@echo Makefile
	@echo
	@echo --- TARGETS ---
	@grep -F -h "##" $(MAKEFILE_LIST) | grep -F -v fgrep | sed -e 's/\\$$//' | sed -e 's/##//'

bootstrap:                       ## bootstrapping the environment
	echo "currently nothing"

clean:                           ## clean up everything
	rm -fr $(BUILD_DIR)

download:                        ## download the dependencies
ifneq ($(wildcard $(DOWNLOAD_FILE)),)
	@echo "already downloaded!"
else
	mkdir -p $(BUILD_DIR)
	wget https://cdn.kernel.org/pub/linux/kernel/v$(KERNEL_MAJOR_VERSION).x/linux-$(KERNEL_VERSION).tar.xz -O $(DOWNLOAD_FILE)
endif

extract: download                ## extract dependencies
ifneq ($(wildcard $(EXTRACT_DIR)),)
	@echo "already extracted!"
else
	mkdir -p $(BUILD_DIR)
	tar xf $(DOWNLOAD_FILE) -C $(BUILD_DIR)
endif

build: extract                   ## build kernel
	cd $(EXTRACT_DIR) && make x86_64_defconfig && make $(MAKEFLAGS) && du -shc arch/x86/boot/bzImage

package: build                   ## package kernel
	@echo "nothing to do!"

