---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.5.2. UF2 format details
pages: 401-401
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.5.2. UF2 format details

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

5.5. USB mass storage interface
400
