BUILD_DIR := build_dir
SRCS := init.c
OBJS := $(addprefix $(BUILD_DIR)/,$(SRCS:.c=.o))
EXE := $(BUILD_DIR)/init
LDFLAGS := -static


help:                            ## printing out the help
	@echo
	@echo Makefile
	@echo
	@echo --- TARGETS ---
	@grep -F -h "##" $(MAKEFILE_LIST) | grep -F -v fgrep | sed -e 's/\\$$//' | sed -e 's/##//'


bootstrap:                       ## bootstrapping the virtualenv
	echo "currently nothing"

download:                        ## download the dependencies
	mkdir -p download
	echo "nothing to download"

clean:                           ## clean up everything
	rm -fr $(BUILD_DIR)

build: $(EXE)                     ## build kernel

$(BUILD_DIR):
	mkdir -p $@

$(BUILD_DIR)/%.o: %.c | $(BUILD_DIR)
	$(CC) $(CFLAGS) $(CPPFLAGS) -c -o $@ $<

$(EXE) : $(OBJS)
	echo lala
	/bin/bash -c pwd
	$(CC) -o $@ $^ $(LDFLAGS) $(LDLIBS)
	find $(BUILD_DIR) | cpio -o -H newc | gzip > $(BUILD_DIR)/root.cpio.gz

run:                             ## run 
	./$(EXE)






