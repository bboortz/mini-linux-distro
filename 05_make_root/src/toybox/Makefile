BUILD_DIR                  := build_dir
KERNEL_VERSION             ?= 6.1.47
TOYBOX_VERSION             ?= 0.8.10
DOWNLOAD_FILE              := $(BUILD_DIR)/toybox-$(TOYBOX_VERSION)
EXTRACT_DIR                := $(BUILD_DIR)/toybox-$(TOYBOX_VERSION)
NPROCS                     := $(shell grep -c ^processor /proc/cpuinfo)
NPROCS                     := $(shell echo "$(NPROCS)-2"|bc)
CC                         := musl-gcc
LDFLAGS                    := --static
#MAKEFLAGS                  := -j $(NPROCS) CC=$(CC)


help:                            ## printing out the help
	echo $(MAKE)
	echo $(MAKEFLAGS)
	@echo
	@echo Makefile
	@echo
	@echo --- TARGETS ---
	@grep -F -h "##" $(MAKEFILE_LIST) | grep -F -v fgrep | sed -e 's/\\$$//' | sed -e 's/##//'

bootstrap:                       ## bootstrapping the environment
	echo "nothing to bootstrap!"

clean:                           ## clean up everything
	rm -fr $(BUILD_DIR)

download:                        ## download the dependencies
ifneq ($(wildcard $(DOWNLOAD_FILE)),)
	@echo "already downloaded!"
else
	mkdir -p $(BUILD_DIR) 
	cd $(BUILD_DIR) && git clone -b $(TOYBOX_VERSION) https://github.com/landley/toybox.git toybox-$(TOYBOX_VERSION)
#	wget http://www.landley.net/toybox/downloads/toybox-$(TOYBOX_VERSION).tar.gz -O $(DOWNLOAD_FILE)
endif

extract: download                ## extract dependencies
	@echo "nothing to extract!"

build: extract                   ## build everything
	cd $(EXTRACT_DIR) && $(MAKE) CC=$(CC) LDFLAGS=$(LDFLAGS) $(MAKEFLAGS) defconfig 
	cd $(EXTRACT_DIR) && $(MAKE) CC=$(CC) LDFLAGS=$(LDFLAGS) $(MAKEFLAGS) 
	cd $(EXTRACT_DIR) && $(MAKE) CC=$(CC) LDFLAGS=$(LDFLAGS) $(MAKEFLAGS) install
	cd $(EXTRACT_DIR) && du -sh install/bin/toybox && file install/bin/toybox 

package: build                   ## package everything
	cd $(EXTRACT_DIR) && make CC=$(CC) root LINUX=../linux/build_dir/linux-$(KERNEL_VERSION)
	cd $(EXTRACT_DIR) && du -sh root/host/initramfs.cpio.gz && file root/host/initramfs.cpio.gz

