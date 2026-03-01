---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.4. Image definitions
pages: 356-356
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 5.1.4. Image definitions

RP2350 Datasheet

5.1.2.1. Partition attributes

Each partition specifies partition attributes for the flash addresses it encompasses, including:

• Start/end offsets within the logical 32 MB address space of the two flash windows; these offsets are specified in

multiples of a flash sector (4 kB)

◦Bootable partitions must reside wholly in the first 16 MB flash window, due to limitations of the address

translation hardware
• Access permissions for the partition: read/write for each of Secure (S), Non-secure (NS) and bootloader (BL) access
• Information on which UF2 family IDs may be dropped into the partition via the UF2 bootloader
• An optional 64-bit identifier
• An optional name (a string for human-readable identification)
• Whether to ignore the partition during Arm or RISC-V boot
• Information to group partitions together (see Section 5.1.7 and Section 5.1.18)

Section 5.9.4 documents the full list of partition attributes, along with the PARTITION_TABLE binary format.

If there is no partition table, the entirety of flash is considered a single region, with no restricted permissions. Without a

partition table, there is no support for custom UF2 family IDs, therefore you must use one of the standard IDs specified

in Table 455.

5.1.3. Flash permissions

One of the roles of the partition tables introduced in Section 5.1.2 is to define flash permissions, or simply permissions.

The partition table stores one set of permission flags for each partition: all bytes covered by a single partition have the

same permissions. The partition table separately defines permissions for unpartitioned space: flash addresses which

do not match any of partitions defined in the partition table.

Separate read/write permissions are specified for each of Secure (S), Non-secure (NS) and bootloader (BL) access.

Bootloader permissions control where UF2s can be written to, and what can be accessed via picotool when the device is

in BOOTSEL mode.

Because flash permissions may be changed dynamically at runtime, part of the partition table is resident in RAM at

runtime. You can modify this table to add permissions for other areas of flash at runtime, without changing the partition

table stored in flash itself. There is no bootrom API for this, however the in-memory partition table format is

documented, and a pointer is available in the ROM table. The SDK provides APIs to wrap this functionality.

5.1.4. Image definitions

An image is a contiguous data blob which may contain code, or data, or a mixture. An image definition is a block of

metadata embedded near the start of an image. The metadata stored in the image definition allows the bootrom to

recognise valid executable and non-executable images. The IMAGE_DEF data structure represents the image definition in a

binary format, and is an example of a block.

For executable images, the IMAGE_DEF could be considered similar to an ELF header, as it can include image attributes

such as architecture/chip, entry-point, load addresses, etc.

All IMAGE_DEFs can contain version information and be hashed or signed. Whilst the bootrom only directly boots

executable images, it does provide facilities for selecting a valid (possibly signed) data image from one or more

partitions on behalf of a user application.

The presence of a valid IMAGE_DEF allows the bootrom to discern a valid application in flash from random data. As a

result, you must include a valid IMAGE_DEF in any executable binary that you intend to boot.

For more information about how the bootrom discovers IMAGE_DEFs, see the section on block loops.

5.1. Bootrom concepts
355
