---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.2. Partition tables
pages: 355-356
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 5.1.2. Partition tables

5.1.2. Partition tables

A partition table divides flash into a maximum of 16 distinct regions, known as partitions. Each partition defines

attributes such as flash permissions for a contiguous range of flash addresses. The PARTITION_TABLE data structure

describes a partition table, and is an example of a block. Use of partition tables is strictly optional.

Dividing flash into multiple partitions enables you to:

• Store more than one executable image on the device. For example:

◦For A/B boot versions (Section 5.1.7)

◦For different architectures (Arm/RISC-V) or Secure/Non-secure

◦For use with a custom bootloader
• Provision space for data. For example:

◦Embedded file systems

◦Shared Wi-Fi firmware

◦Application resources
• Provide different security attributes for different regions of flash (Section 5.1.3)
• Target UF2 downloads to different partitions based on family ID (Section 5.1.18), including custom-defined UF2

families specific to your platform

For more information about PARTITION_TABLE discovery during flash boot, see Section 5.1.5.2.

Partition tables can be versioned to support A/B upgrades. They can also be hashed and signed for security and

integrity purposes. We recommend hashing partition tables to ensure that they haven’t been corrupted. This is

especially important when using boot slots to update your partition table, since a corrupted partition table with a higher

version could be chosen over a non-corrupted partition table with a lower version.

5.1. Bootrom concepts
354

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
