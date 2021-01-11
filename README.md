chipKIT Compiler
================

This repository contains scripts, configuration files, and instructions for
building the chipKIT compiler.

Much of the build process is automated.

Building the base toolchain
---------------------------

Step one: clone this repository and the submodules:

```
git clone --recurse-submodules https://github.com/MajenkoProjects/chipkit-compiler
```

Alternatively you can get the two required repositories manually:

* https://github.com/MajenkoProjects/pic32-genfiles
* https://github.com/MajenkoProjects/crosstool-ng-pic32

Step two: compile crosstool-ng:

```
cd chipkit-compiler/crosstool-ng-pic32
./bootstrap
./configure
make
```

Step three: build the toolchain for your desired OS. If you only want it to run on your current
operating system then simpluy run:

```
./ct-ng build-mipsel-pic32-elf
```

However if you want to cross-compile to run on another OS then run:

```
./ct-ng build-<os triplet>,mipsel-pic32-elf
```

Currently supported OS triplets: 

* `x86_64-linux-gnu`
* `i686-linux-gnu`
* `arm-linux-gnueabihf`
* `aarch64-linux-gnu`
* `x86_64-w64-mingw32`

And on OS X only (see [this blog post](https://majenko.co.uk/blog/how-i-cross-compile-fat-binary-cross-compiler-os-x-big-sur)):

* `arm64e-apple-darwin20`
* `x86_64-apple-darwin20`

So as an example you would run:

```
./ct-ng build-arm-linux-gnueabihf,mipsel-pic32-elf
```

If compiling for Windows (`x86_64-w64-mingw32`) you will get errors concerning `_bfd_error_handler`. These are safe to ignore, they
are actually only a warning, not an error. 

This will build the base toolchain in `~/x-tools/` either as `mipsel-pic32-elf` or as `HOST-<triplet>/mipsel-pic32-elf`.

Adding pic32 support files
--------------------------

For this make sure that the `pic32-genfiles` sub-module is checked out (or
clone it separately from [here](https://github.com/MajenkoProjects/pic32-genfiles)).

From within that directory run:

```
PATH=~/x-tools/mipsel-pic32-elf/bin:${PATH} bin/pic32-genfiles
```

This will build the support files and libraries for all supported PIC32 chips.

Next copy the files to the toolchain. If you didn't cross-compile, that's:

```
cp -R lib include ~/x-tools/mipsel-pic32-elf/mipsel-pic32-elf/
cp src/cp0defs.h ~/x-tools/mipsel-pic32-elf/mipsel-pic32-elf/include
```

Otherwise insert the HOST directory, for example:

```
cp -R lib include ~/x-tools/HOST-arm-linux-gnueabihf/mipsel-pic32-elf/mipsel-pic32-elf/
cp src/cp0defs.h ~/x-tools/HOST-arm-linux-gnueabihf/mipsel-pic32-elf/mipsel-pic32-elf/include
```

The toolchain should now be complete and ready to go.
