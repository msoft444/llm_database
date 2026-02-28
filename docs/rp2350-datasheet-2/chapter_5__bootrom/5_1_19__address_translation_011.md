---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.19. Address translation
pages: 364-364
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 5.1.19. Address translation

RP2350 Datasheet

2. If there is no partition table, then the data, rp2350-arm-s (if Arm is enabled) and rp2350-riscv (if RISC-V is enabled)

family IDs are allowed by default. The UF2 is always downloaded to the start of flash.

3. If there is a partition table, then non-absolute family IDs target a single partition under the control of the partition

table:

a. A UF2 will not be downloaded to a partition that doesn’t have BL-write flash permissions

b. Each partition lists which family IDs it accepts (both RP2350 standard and user defined)

c. With A/B partitions; the A partition indicates the family IDs supported, and the UF2 goes to the partition that

isn’t the currently booting one (strictly the one that won’t be the one chosen if the device were rebooted now).

d. Further refinement with A/B is allowed to support secondary A/B partitions containing data/executables used

(owned) by the main partitions; see Section 5.1.18.1 for detailed information.

For details of the exact rules used when picking a UF2 target partition, see Section 5.5.3.

NOTE

UF2 family ids are used for partition targeting when copying UF2s to the USB drive, or when using picotool load -p.

When using picotool load without the -p flag images can be written anywhere in flash that has BL-write permissions.

5.1.18.1. Owned partitions

An executable might require data from another partition (such as Wi-Fi firmware). When the main executable is stored in

A/B partitions, for safe upgrades, it may be desirable to associate two other partitions C and D with the primary A and B

partitions, such that:

• the data in partition C is used for executable in partition A, and
• the data in partition D is used for the executable in partition B.

In this scenario A is marked as the owner of C in the partition table, and C is A’s owned partition. This affects UF2

image downloads which (due to their UF2 family ID) target partitions C and D.

When a UF2 download targets the C/D partition pair, the bootrom checks the state of the A and B owning partitions to

determine which of the owned partitions (C and D) receives the download. By default:

• If B would be the target partition for a UF2 with an A/B-compatible family ID, then D is the target for a UF2 with the

C/D compatible family ID.
• Conversely, when A is the target partition for A/B downloads, C is the target partition for C/D downloads.

The FLAGS_UF2_DOWNLOAD_AB_NON_BOOTABLE_OWNER_AFFINITY flag in the partition table reverses this mapping.

5.1.19. Address translation

RP2040 required images to be stored at the beginning of flash (0x10000000). RP2350 supports storing executable images

in a partitions at arbitrary locations, to support more robust upgrade cycles via A/B versions, among other uses. This

presents the issue that the address an executable is linked at, and therefore the binary contents of the image, would

have to depend on the address it is stored at. This can be worked around to an extent with position-independent code,

at cost to code size and performance.

RP2350 avoids this pitfall with hardware and bootrom support for address translation. An image stored at any 4 kB-

aligned location in flash can appear at flash address 0x10000000 at runtime. The SDK continues to assume an image base

of 0x10000000 by default.

When launching an image from a partition, the bootrom initialises QMI registers ATRANS0 through ATRANS3 to map a

flash runtime address of 0x10000000 (by default) to the flash storage address of the start of the partition. It sets the total

size of the mapped region to the size of the partition, with a maximum size of 16 MB. Accessing flash addresses

beyond the size of the booted partition (but below the 0x11000000 chip select watermark) returns a bus fault.

5.1. Bootrom concepts
363
