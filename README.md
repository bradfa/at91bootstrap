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

## Write at91bootstrap into NAND

To write the at91bootstrap executable into NAND flash, use a JTAG tool such
as the Olimex ARM-USB-OCD-H or Tincan Tools Flyswatter2 along with OpenOCD.
Erase the first block of NAND flash, then write the at91boostrap binary to
it.  For example, in OpenOCD on the Olimex SAM9-L9260 board:

1. Start OpenOCD:

    openocd -f interface/olimex-arm-usb-ocd-h.cfg -f board/olimex_sam9_l9260.cfg -c init -c "reset init"

2. Telnet into OpenOCD:

    telnet localhost 4444

3. Enable fast memory access and raw NAND access:

    arm7_9 fast_memory_access enable
    nand raw_access 0 enable

4. Erase the first block of NAND, then write the at91bootstrap binary:

    nand erase 0 0x0 0x20000
    nand write 0 /home/andrew/calc/at91bootstrap/binaries/sam9_l9260-nandflashboot-2.4.bin 0x0

5. Then write u-boot to NAND address 0x20000, that's where at91boostrap will
copy from in order to load a second stage bootloader:

    nand write 0 /home/andrew/calc/u-boot/u-boot.bin 0x20000

