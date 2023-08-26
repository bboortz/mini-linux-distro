BUILD_DIR := build_dir
EXE := ./init
CC := "musl-gcc"
LDFLAGS := -static

.PHONY: help bootstrap clean download extract build package run


help:                            ## printing out the help
	@echo
	@echo Makefile
	@echo
	@echo --- TARGETS ---
	@grep -F -h "##" $(MAKEFILE_LIST) | grep -F -v fgrep | sed -e 's/\\$$//' | sed -e 's/##//'


bootstrap:                       ## bootstrapping the virtualenv
	echo "currently nothing"

clean:                           ## clean up everything
	rm -fr $(BUILD_DIR)

download:                        ## download the dependencies
	@echo "nothing to download"

extract:                         ## extract the dependencies
	@echo "nothing to extract"

build:                           ## build 
	@echo "nothing to build"
	mkdir -p $(BUILD_DIR)
	cp init $(BUILD_DIR)

run:                             ## run 
	./$(EXE)






