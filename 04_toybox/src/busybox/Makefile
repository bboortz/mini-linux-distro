BUILD_DIR                  := build_dir
BUSYBOX_VERSION            ?= 1.36.1
DOWNLOAD_FILE              := $(BUILD_DIR)/busybox-$(BUSYBOX_VERSION).tar.bz2
EXTRACT_DIR                := $(BUILD_DIR)/busybox-$(BUSYBOX_VERSION)
NPROCS                     := $(shell grep -c ^processor /proc/cpuinfo)
NPROCS                     := $(shell echo "$(NPROCS)-2"|bc)
CC                         := musl-gcc
MAKEFLAGS                  := -j $(NPROCS) CC=$(CC)


help:                            ## printing out the help
	echo $(MAKE)
	echo $(MAKEFLAGS)
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
	wget https://www.busybox.net/downloads/busybox-$(BUSYBOX_VERSION).tar.bz2 -O $(DOWNLOAD_FILE)
endif

extract: download                ## extract dependencies
ifneq ($(wildcard $(EXTRACT_DIR)),)
	@echo "already extracted!"
else
	mkdir -p $(BUILD_DIR)
	tar xf $(DOWNLOAD_FILE) -C $(BUILD_DIR)
endif

build: extract                   ## build kernel
	cd $(EXTRACT_DIR) && $(MAKE) CC=$(CC) $(MAKEFLAGS) defconfig && sed -i -e "s/# CONFIG_STATIC is not set/CONFIG_STATIC=y/" .config && grep CONFIG_STATIC .config 
	cd $(EXTRACT_DIR) && $(MAKE) CC=$(CC) $(MAKEFLAGS) && $(MAKE) CC=$(CC) $(MAKEFLAGS) CONFIG_PREFIX=BUSYBOX install
	cd $(EXTRACT_DIR) && du -sh BUSYBOX/bin/busybox && file BUSYBOX/bin/busybox 

package: build                   ## package kernel
	@echo "nothing to do!"

