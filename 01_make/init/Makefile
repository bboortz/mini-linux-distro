BUILD_DIR := build_dir
SRCS := init.c
OBJS := $(addprefix $(BUILD_DIR)/,$(SRCS:.c=.o))
EXE := $(BUILD_DIR)/init
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

build: $(EXE)                    ## build 

$(BUILD_DIR):
	mkdir -p $@

$(BUILD_DIR)/%.o: %.c | $(BUILD_DIR)
	$(CC) $(CFLAGS) $(CPPFLAGS) -c -o $@ $<

$(EXE) : $(OBJS)
	$(CC) -o $@ $^ $(LDFLAGS) $(LDLIBS)

package: build                   ## package everything
	cd $(BUILD_DIR) && find . | cpio -o -H newc | gzip > root.cpio.gz

run:                             ## run 
	./$(EXE)






