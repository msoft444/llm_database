---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.5.2. UF2 format details
pages: 401-401
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.5.2. UF2 format details

• INFO_UF2.TXT contains a string description of the UF2 bootloader and version.
• INDEX.HTM redirects to information about the RP2350 device.
The default INDEX.HTM for RP2350 A2 is https://raspberrypi.com/device/RP2?version=5A09D5312E22. The version
parameter is changed for other RP2350 revisions; the first 6 characters are from the git hash of the chip, and the next 6
characters are from the git hash of the bootrom. The contents of these files and the name of the drive may be
customised. For more information, see Section 5.7.
Any type of files may be written to the USB drive from the host; however, in general these aren’t stored, and only appear
to be so because of caching on the host side.
When a suitable UF2 file is written to the device, the special contents are recognised and data is written to specified
locations in RAM or flash.
Where flash-targeted UF2s are written on RP2350 is determined by the family id of the UF2 contents and the partition
table.
If there’s no partition table, then UF2s are stored at the address they specify; otherwise they (with the exception of the
special ABSOLUTE family id) are stored into a single partition, with UF2 flash address 0x10000000 mapping to the start of
the partition.
It’s possible, based on the partition table or family id, that the UF2 isn’t downloadable anywhere in flash, in which case,
it’s ignored. Further detail can be discovered via GET_INFO - UF2_STATUS. On the completed download of an entire
valid UF2 file, RP2350 automatically reboots to run the newly downloaded code.
Invalid UF2 files might not write at all or only write partially to RP2350 before failing. Not all operating systems notify
you of disk write errors after a failed write. You can use picotool verify to verify that a UF2 file wrote correctly to
RP2350.
5.5.2. UF2 format details
This section describes the constraints on a UF2 file to be valid for download.

TIP
To generate UF2 files, you can use the picootol uf2 convert functionality in picotool.
All data destined for the device must be in UF2 blocks with:
• A familyID present, with a value in the reserved range 0xe48bff58 through 0xe48bff5b or a user family ID configured in
a partition table (see table in Section 5.5.3).
• A payload_size of 256.
All data must be destined for (and fit entirely within) the following memory ranges (depending on the type of binary
being downloaded which is determined by the address of the first UF2 block encountered):
• A regular flash image
◦0x10000000-0x12000000 flash: All blocks must be targeted at 256 byte alignments. Writes beyond the end of
physical flash will wrap back to the beginning of flash.
• A RAM only image
◦0x20000000-0x20082000 main RAM: Blocks can be positioned with byte alignment.
◦0x13ffc000-0x14000000 XIP RAM: (since flash isn’t being targeted, the flash cache is available for use as RAM
with same properties as main RAM).
RP2350 Datasheet
5.5. USB mass storage interface
400

