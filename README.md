AT91-Bootstrap or similar is needed if you want to load U-Boot
from Dataflash or NAND flash.  Note that this is an 
unofficial version, and is not supported by Atmel.

The major change Olimex have made is to add a few subroutines
to allow the at91-bootstrap to build without a C library.
(memset,memcpy,udiv) - udiv() is an unsigned version of div()

Previous versions had to be built using a statically linked 
C library, making it unfriendly to buildroot, as well as
to the "supported" TimeSys gcc compilers.

If you use this version, it should be able to be built by any
gcc compiler including the Emdebian toolchain, those found at
www.timesys.com and the compilers generated by buildroot.

The Makefile has undergone drastic changes to allow easy use.

Instead of walking down a directory tree and make 
the file at the bootom of the tree,
you configure AT91-Bootstrap before use

----------------------------------------------------------
    $ make CROSS_COMPILE=arm-linux-gnueabi- MEMORY=nandflash sam9_l9260_defconfig
    $ make

should be enough to build the NAND flash version for Olimex
L9260 board.

----------------------------------------------------------
at91-bootstrap supports

* at9sam9260ek	dataflash and NAND flash
* at9sam9261ek	dataflash and NAND flash
* at9sam9263ek	dataflash

----------------------------------------------------------
If you want a toolchain that builds at91-bootstrap you should use
the [Emdebian][1] toolchain.

[1]: http://wiki.debian.org/EmdebianToolchain

