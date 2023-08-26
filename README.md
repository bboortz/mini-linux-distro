# mini-linux-distro
a minimal linux distribution for fun and education in order to learn how to build and bootstrap a "minimal linux distribution"

# directory structure
* 00_hello_world - the first try with a linux kernel and simple init program
* 01_make - an improved make process to streamline the build with the same kernel and init program
* 02_busybox - exchanging the init program with a busybox statically build binary
* 03_toybox - samelike 02_busybox but with toybox


# usage

```
cd 01_make
make help
make package
make run
```


# dependencies 

## which you have to provide to build everything

* some linux / docker container / ...
* make
* musl-libc


## which will be downloaded

* linux
* busybox
* toybox


