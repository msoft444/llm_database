---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.9.4. Partition table items
pages: 426-429
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 5.9.4. Partition table items

5.9.4. Partition table items

Partition tables allows dividing the 32 MB flash region (2 × 16 MB) into partitions. Permissions and other partition

attributes may be specified for each partition, along with permissions for the un-partitioned space.

The permission specify read/write access for Secure code, Non-secure code, and "NSBoot" which refers to the boot

loader (and PICOBOOT)

NOTE

These permissions are only advisory to Secure code, however they are respected by flash_op(), the PICOBOOT flash

access commands, and UF2 downloads.

5.9.4.1. PARTITION_TABLE item

| Word | Bytes | Value |
| --- | --- | --- |
| 0 | 1 | 0x44 (size_flag == 0, item_type == PICOBIN BLOCK ITEM PARTITION TABLE) _ _ _ _ |
|  | 2 | Block size in words |
|  | 1 | singleton_flag:1, pad:3 , partition_count:4 |
| 1 | 4 | unpartitioned_space_permissions_and_flags |
| Partition 0 |  |  |
| 2 | 4 | permissions_and_location for partition 0 |
| 3 | 4 | permissions_and_flags for partition 0 |
|  | if _partition | _0_has_id: |
| 3 | 4 | partition_0_ID_lo |
| 4 | 4 | partition_0_ID_hi |
|  | one word p | er additional family ID (can be none): |
| x | 4 | partition_0_family ID_0 |
| x + 1 | 4 | partition_0_family ID_1 |
|  | … | … |
|  | if _partition | _0_has_name: |
| y | 1 | reserved:1 (0), name_len_bytes:7 |
|  | 1 | partition_0_name_byte_0 |
|  | 1 | partition_0_name_byte_1 |
|  | 1 | partition_0_name_byte_2 |
| y+1 | 1 | partition_0_name_byte_3 |
|  | 1 | partition_0_name_byte_4 |
|  | 1 | partition_0_name_byte_5 |
|  | 1 | partition_0_name_byte_6 |
| … | … | … |
|  | ? | partition_0_name_byte_n_minus_x to partition_0_name_byte_n_minus_2 |
|  | 1 | partition_0_name_byte_n_minus_1 |
|  | ? | (padding zero bytes to reach word alignment) |
| (Partition 1 | ) |  |
| … | … | … |

5.9. Metadata block details
425

RP2350 Datasheet

5.9.4.2. Partition location, permissions, and flags

Two common words are stored in the partition table for both un-partitioned space and each partition. These common

words describe the size/location, along with access permissions and various flags.

The permission fields are repeated in both words, hence the two words are permissions_and_location and

permissions_and_flags.

| Mask | AppliesTo | Description |
| --- | --- | --- |
| 0x04000000u | 'P' 'U' | PERMISSION S R BITS _ _ _ |
|  |  | If set, the partition is readable by Secure code. See Section 5.1.3 |
| 0x08000000u | 'P' 'U' | PERMISSION S W BITS _ _ _ |
|  |  | If set, the partition is writable by Secure code. See Section 5.1.3 |
| 0x10000000u | 'P' 'U' | PERMISSION NS R BITS _ _ _ |
|  |  | If set, the partition is readable by Non-secure code. See Section 5.1.3 |
| 0x20000000u | 'P' 'U' | PERMISSION NS W BITS _ _ _ |
|  |  | If set, the partition is writable by Non-secure code. See Section 5.1.3 |
| 0x40000000u | 'P' 'U' | PERMISSION NSBOOT R BITS _ _ _ |
|  |  | If set, the partition is readable by NSBOOT (boot loader) Secure code. See Section 5.1.3 |
| 0x80000000u | 'P' 'U' | PERMISSION NSBOOT W BITS _ _ _ |
|  |  | If set, the partition is writable by NSBOOT (boot loader) Secure code. See Section 5.1.3 |

*Table 472. Permission Fields. 'P' means the field applies to partitions, 'U' means the field applies to un- partitioned space, however the word "partition" is always used in the description*

| Mask | AppliesTo | Description |
| --- | --- | --- |
| 0x00001fffu | 'P' 'U' | LOCATION FIRST SECTOR BITS _ _ _ |
|  |  | The sector number (0-4095) of the first sector in the partition (a sector is 4 kB) |
| 0x03ffe000u | 'P' 'U' | LOCATION LAST SECTOR BITS _ _ _ |
|  |  | The sector number (0-4095) of the last sector in the partition (a sector is 4 kB) |
| 0x00000001u | 'P' | FLAGS HAS ID BITS _ _ _ |
|  |  | If set, the partition has a 64 bit identifier |
| 0x00000006u | 'P' | FLAGS LINK TYPE BITS _ _ _ |
|  |  | The type of link stored in the partition: • 0x0 - None • 0x1 - A PARTITION : This is a "B" partition, and The LINK VALUE field stores the partition _ _ number of the corresponding "A" partition • 0x2 - OWNER : This is an "A" partition, and the LINK VALUE field stores the partition number of _ the owning partition (which should also be an "A" partition). |
| 0x00000078u | 'P' | FLAGS LINK VALUE BITS _ _ _ |
|  |  | If LINK TYPE is non zero, then this field holds the partition number of the linked partition. _ |
| 0x00000180u | 'P' | FLAGS ACCEPTS NUM EXTRA FAMILIES BITS _ _ _ _ _ |
|  |  | 0-3 the number of extra non-standard UF2 family ids the partition accepts. |
| 0x00000200u | 'P' | FLAGS NOT BOOTABLE ARM BITS _ _ _ _ |
|  |  | If set then this partition is marked non-bootable on Arm, and will be ignored during Arm boot. Setting this for non Arm bootable partitions can improve boot performance. |
| 0x00000400u | 'P' | FLAGS NOT BOOTABLE RISCV BITS _ _ _ _ |
|  |  | If set then this partition is marked non-bootable on RISC-V, and will be ignored during RISC-V boot. Setting this for non RISC-V bootable partitions can improve boot performance. |
| 0x00000800u | 'P' | FLAGS UF2 DOWNLOAD AB NON BOOTABLE OWNER AFFINITY _ _ _ _ _ _ _ |
|  |  |  |
| 0x00001000u | 'P' | FLAGS HAS NAME BITS _ _ _ |
|  |  | If set, the partition has a name. |
| 0x00002000u | 'P' 'U' | FLAGS UF2 DOWNLOAD NO REBOOT BITS _ _ _ _ _ |
|  |  | If set, the RP2350 will not reboot after dragging a UF2 into this partition. |
| 0x00004000u | 'P' 'U' | FLAGS ACCEPTS DEFAULT FAMILY RP2040 BITS _ _ _ _ _ |
|  |  | If set, a UF2 with the RP2040 family id 0xe48bff56 may be downloaded into this partition. |
| 0x00008000u | 'U' | FLAGS ACCEPTS DEFAULT FAMILY ABSOLUTE BITS _ _ _ _ _ |
|  |  | If set for un-partitioned spaced, a UF2 with the ABSOLUTE family id 0xe48bff57 may be downloaded onto the RP2350 and will be written at the addresses specified in the UF2 without regard to partition locations. Partition-defined flash access permissions are still respected (the UF2 download will fail if it needs to write over a read-only region of flash). |
| 0x00010000u | 'P' 'U' | FLAGS ACCEPTS DEFAULT FAMILY DATA BITS _ _ _ _ _ |
|  |  | If set, a UF2 with the DATA family id 0xe48bff58 may be downloaded into this partition. |
| 0x00020000u | 'P' 'U' | FLAGS ACCEPTS DEFAULT FAMILY RP2350 ARM S BITS _ _ _ _ _ _ _ |
|  |  | If set, a UF2 with the RP2350 ARM S family id 0xe48bff59 may be downloaded into this partition. _ _ |
| 0x00040000u | 'P' 'U' | FLAGS ACCEPTS DEFAULT FAMILY RP2350 RISCV BITS _ _ _ _ _ _ |
|  |  | If set, a UF2 with the RP2350 RISC V family id 0xe48bff5a may be downloaded into this partition. _ _ |
| 0x00080000u | 'P' 'U' | FLAGS ACCEPTS DEFAULT FAMILY RP2350 ARM NS BITS _ _ _ _ _ _ _ |
|  |  | If set, a UF2 with the RP2350 ARM NS family id 0xe48bff5b may be downloaded into this partition. _ _ |
| 0x03f00000u | 'P' 'U' | reserved; should be 0 |

*Table 473. Location Fields. 'P' means the field applies to partitions, 'U' means the field applies to un- partitioned space, however the word "partition" is always used in the description*

5.9. Metadata block details
426

RP2350 Datasheet

*Table 474. Flags Fields. 'P' means the field applies to partitions, 'U' means the field applies to un- partitioned space, however the word "partition" is always used in the description*

5.9. Metadata block details
427

RP2350 Datasheet
