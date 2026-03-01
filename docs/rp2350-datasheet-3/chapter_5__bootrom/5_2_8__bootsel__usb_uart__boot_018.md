---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2.8. BOOTSEL (USB/UART) boot
pages: 375-375
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 5.2.8. BOOTSEL (USB/UART) boot

![Page 375 figure](images/fig_p0375.png)

RP2350 Datasheet

Flash image boot

A flash image can be written directly to flash storage address 0x0, and the bootrom will find it from there. This is the

most similar to flash boot on RP2040 (the main differences being the removal of a boot2 in the first 256 bytes of the

image, and the new requirement for a valid image definition anywhere within the first 4 kB of the image).

Flash partition boot

A flash image can be written into a partition of a partition table. The partition table is described by a PARTITION_TABLE

block stored at the start of flash. The bootrom finds the partition table and scans its partitions to look for bootable

images.

Partition-table-in-image boot

A flash image containing an IMAGE_DEF and PARTITION_TABLE block in a single block loop is written to the start of flash.

The bootrom loads the embedded partition table, and enters the image in the same way as the flash image boot

case.

Revisit the linked bootrom concepts sections to get the fullest understanding of each of these three forms of flash boot.

For the purposes of this section, all that matters is whether the bootrom can discover a valid, bootable image or not. In

all three cases, the image must have a valid IMAGE_DEF, and meet all relevant security requirements such as being

correctly signed, and having a rollback version greater than or equal to the one stored in OTP.

The bootrom enters the flash image in whatever QSPI mode it discovered to work during flash programming. Any

further setup (such as prefixless continuous read modes) is performed by the flash image itself. This setup code,

referred to as an XIP setup function, is usually copied into RAM before execution to avoid running from flash whilst the

XIP interface is being reconfigured.


TIP

The PICO_EMBED_XIP_SETUP=1 flag in the SDK enables inclusion and execution of an XIP setup function on RP2350

builds. In this case the function executes on the core 0 stack during early startup, so no additional static memory

need be allocated. This is not the case for subsequent calls, because the stack is often not executable post-startup.

You should save your XIP setup function in the first 256 bytes of boot RAM to make it easily locatable when the XIP

mode is re-initialised following a serial flash programming operation which had to drop out of XIP mode. The bootrom

writes a default XIP setup function to this address before entering the flash image, which restores the mode the

bootrom discovered during flash programming.

NOTE

You cannot execute an XIP setup function directly from boot RAM, because boot RAM is never executable. You must

copy it into SRAM before execution.

XIP setup functions should be fully position-independent, and no more than 256 bytes in size. If you are unable to meet

these requirements, you should install a stub function which calls your XIP setup function elsewhere in RAM.

5.2.8. BOOTSEL (USB/UART) boot

The bootrom samples the state of QSPI CSn shortly after reset. Based on the result, the bootrom decides whether to

enter BOOTSEL mode, which refers collectively to the USB and UART bootloaders.

The bootrom initialises the chip select to the following state:

• Output disabled
• Pulled high (note CSn is an active-low signal, so this deselects the external QSPI device if there is one)

If the chip select remains high, the bootrom continues with its normal, non-BOOTSEL sequence. By default on a blank

device, this means driving the chip select low and attempting to boot from an external flash or PSRAM device.

If chip select is driven low externally, the bootrom enters BOOTSEL mode. You must drive the chip select low with a

5.2. Processor-controlled boot sequence
374
