---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.15. Flash boot slots
pages: 361-361
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 5.1.15. Flash boot slots

RP2350 Datasheet

Flash image boot has no partition table, so it cannot be used with A/B version checking, which requires separate A/B

partitions. The IMAGE_DEF will boot if it is valid (which includes requiring a signature on a secured RP2350).

For the non-signed case, the IMAGE_DEF can be as small as a 20-bytes; see Section 5.9.5.


TIP

A more complicated version of this scenario stores multiple IMAGE_DEFs in the block loop. In this case, the last

IMAGE_DEF for the current architecture is booted, if valid. You can use this to implement universal binaries for various

supported architectures, or to include multiple signatures for targeting devices with different keys.

5.1.13. Flash partition boot

If a PARTITION_TABLE, but no IMAGE_DEF, is found in the valid block loop that starts within the first 4 kB of flash, and it is valid

(including signature if necessary on a secured RP2350), the bootrom searches that partition table’s partitions for an

executable image to boot. This process, when successful, is referred to as flash partition boot.

The partitions are searched in order, skipping those marked as ignored for the current architecture. The bootrom

ignores partitions as an optimisation, or to prevent automatic architecture switching.

If the partition is not part of an A/B pair, the first 4 kB is searched for the start of a valid block loop. If a valid block loop

is found, and it contains an executable image with a valid (including signature on a secured RP2350) IMAGE_DEF, then that

executable image is chosen for boot.

If the partition is the A partition of an A/B pair, the bootrom searches both partitions as described above. If both

partitions result in a bootable IMAGE_DEF, the IMAGE_DEF with the higher version number is chosen. Otherwise, the valid

IMAGE_DEF is chosen. There are some exceptions to this rule in advanced scenarios; see Section 5.1.16 and Section

5.1.17 for details.

5.1.14. Partition-Table-in-Image boot

If both a PARTITION_TABLE and an IMAGE_DEF block are found in the valid block loop that starts within the first 4 kB of flash, a

third type of flash boot takes place. The IMAGE_DEF and PARTITION_TABLE must only be recognised, not necessarily valid or

correctly signed. This stipulation prevents a causality loop.

This is known as partition-table-in-image boot, since the application contains the partition table (instead of vice versa).

This partition table is referred to as an embedded partition table.

The PARTITION_TABLE is loaded as the current partition table, and the IMAGE_DEF is launched directly. The table defined by

the PARTITION_TABLE is not searched for IMAGE_DEFs to boot.

The following common cases might use this scenario:

• You are only using the PARTITION_TABLE for flash permissions. You want to load that partition table, then boot as

normal.
• The IMAGE_DEF contains a small bootloader stored alongside the partition table. In this case, the partition table will

once again be loaded, and the associated image entered. The entered image will then likely pick a partition from

the partition table, and launch an image from there itself.

5.1.15. Flash boot slots

The previous sections within this chapter discuss block loops starting within the first 4 kB of flash. Such a block loop

contained either an IMAGE_DEF, a partition table (searched for IMAGE_DEFs), or an IMAGE_DEF and a PARTITION_TABLE (not

searched).

All the previously mentioned cases discovered their block loop in slot 0. Under certain circumstances, the neighbouring

slot 1 is also searched.

5.1. Bootrom concepts
360
