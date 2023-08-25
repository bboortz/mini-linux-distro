KERNEL_MAJOR_VERSION       ?= 6
KERNEL_VERSION             ?= 6.1.47


help:                            ## printing out the help
	@echo
	@echo micropython-fun Makefile
	@echo
	@echo --- TARGETS ---
	@grep -F -h "##" $(MAKEFILE_LIST) | grep -F -v fgrep | sed -e 's/\\$$//' | sed -e 's/##//'


bootstrap:                       ## bootstrapping the virtualenv
	echo "currently nothing"

download:                        ## download the dependencies
	mkdir -p download
	wget https://cdn.kernel.org/pub/linux/kernel/v$(KERNEL_MAJOR_VERSION).x/linux-$(KERNEL_VERSION).tar.xz -O download/linux-$(KERNEL_VERSION).tar.xz


clean:                           ## clean up everything
	rm download/*


build: build_kernel build_init   ## build everything

build_init:                      ## build kernel
	cd init && gcc -static init.c -o init && find . | cpio -o -H newc | gzip > root.cpio.gz

build_kernel:                    ## build kernel
	mkdir -p build
	tar xf download/linux-$(KERNEL_VERSION).tar.xz -C build/
	cd build/linux-$(KERNEL_VERION) && make x86_64_defconfig && make -j $(nproc)

run:                             ## build kernel
	qemu-system-x86_64 -nographic -no-reboot -kernel build/linux-6.1.47/arch/x86/boot/bzImage -initrd init/root.cpio.gz -append "panic=1 console=ttyS0"
