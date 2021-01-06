chipKIT Compiler
================

This repository contains scripts, configuration files, and instructions for
building the chipKIT compiler.

Prerequisites
-------------

The main pre-requisite is crosstool-ng.  The github version 
[from here](https://github.com/crosstool-ng/crosstool-ng) is preferred.

You will also need a working PHP cli installation.

Building the base toolchain
---------------------------

The first step is to build the base toolchain.  This is done using crosstool-ng.
The provided `mipsel-pic32-elf.config` file is the crosstool-ng configuration.
Import it into crosstool-ng with:

```
cp mipsel-pic32-elf.config .config
ct-ng oldconfig
```

Then build with:

```
ct-ng build
```

This will build the base toolchain in `~/x-tools/mipsel-pic32-elf`

Adding pic32 support files
--------------------------

For this make sure that the `pic32-genfiles` sub-module is checked out (or
clone it separately from [here](https://github.com/MajenkoProjects/pic32-genfiles)).

From within that directory run:

```
PATH=~/x-tools/mipsel-pic32-elf/bin:${PATH} bin/pic32-genfiles
```

This will build the support files and libraries for all supported PIC32 chips.

Next copy the files to the toolchain:

```
cp -R lib include ~/x-tools/mipsel-pic32-elf/mipsel-pic32-elf/
cp src/cp0defs.h ~/x-tools/mipsel-pic32-elf/mipsel-pic32-elf/include
```

The toolchain should now be complete and ready to go.
