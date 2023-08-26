BUILD_DIR                  := build_dir
TOYBOX_VERSION             ?= 0.8.9
DOWNLOAD_FILE              := $(BUILD_DIR)/toybox-$(TOYBOX_VERSION).tar.gz
EXTRACT_DIR                := $(BUILD_DIR)/toybox-$(TOYBOX_VERSION)
NPROCS                     := $(shell grep -c ^processor /proc/cpuinfo)
NPROCS                     := $(shell echo "$(NPROCS)-2"|bc)
CC                         := musl-gcc
LDFLAGS="--static"
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
	wget http://www.landley.net/toybox/downloads/toybox-$(TOYBOX_VERSION).tar.gz -O $(DOWNLOAD_FILE)
endif

extract: download                ## extract dependencies
ifneq ($(wildcard $(EXTRACT_DIR)),)
	@echo "already extracted!"
else
	mkdir -p $(BUILD_DIR)
	tar xf $(DOWNLOAD_FILE) -C $(BUILD_DIR)
endif

build: extract                   ## build kernel
	cd $(EXTRACT_DIR) && $(MAKE) CC=$(CC) LDFLAGS=$(LDFLAGS) $(MAKEFLAGS) defconfig toybox install
	cd $(EXTRACT_DIR) && du -sh install/bin/toybox && file install/bin/toybox 

package: build                   ## package kernel
	@echo "nothing to do!"

