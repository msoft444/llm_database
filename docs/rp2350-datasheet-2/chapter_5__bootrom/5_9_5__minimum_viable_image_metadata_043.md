---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.9.5. Minimum viable image metadata
pages: 429-429
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 5.9.5. Minimum viable image metadata

![Page 429 figure](images/fig_p0429.png)

RP2350 Datasheet

| Mask | AppliesTo | Description |
| --- | --- | --- |
| 0x00080000u | 'P' 'U' | FLAGS ACCEPTS DEFAULT FAMILY RP2350 ARM NS BITS
_ _ _ _ _ _ _ |
|  |  | If set, a UF2 with the RP2350 ARM NS family id 0xe48bff5b may be downloaded into this partition.
_ _ |
| 0x03f00000u | 'P' 'U' | reserved; should be 0 |

5.9.5. Minimum viable image metadata

A minimum amount of metadata (a valid IMAGE_DEF block) must be embedded in any binary for the bootrom to recognise

it as a valid program image, as opposed to, for example, blank flash contents or a disconnected flash device. This must

appear within the first 4 kB of a flash image, or anywhere in a RAM or OTP image.

Unlike RP2040, there is no requirement for flash binaries to have a checksummed "boot2" flash setup function at flash

address 0. The RP2350 bootrom performs a simple best-effort XIP setup during flash scanning, and a flash-resident

program can continue executing in this state, or can choose to reconfigure the QSPI interface at a later time for best

performance.

5.9.5.1. Minimum Arm IMAGE_DEF

Assuming CRIT1.SECURE_BOOT_ENABLE is clear, the minimum valid IMAGE_DEF is the following 20-byte sequence:

| Word | LE Value | Bytes | Description |
| --- | --- | --- | --- |
| 0 | 0xffffded3 | 4 | PICOBIN BLOCK MARKER START
_ _ _ |
| 1 | 0x10210142 | 1 | 0x42(item_type == PICOBIN_BLOCK_ITEM_1BS_IMAGE_TYPE) |
|  |  | 1 | 0x01 (Item is 1 word in size) |
|  |  | 2 | 0x1021
(PICOBIN IMAGE TYPE IMAGE TYPE AS BITS(EXE) |
_ _ _ _ _ _
PICOBIN IMAGE TYPE EXE SECURITY AS BITS(S) |
_ _ _ _ _ _
PICOBIN IMAGE TYPE EXE CPU AS BITS(Arm) |
_ _ _ _ _ _
PICOBIN IMAGE TYPE EXE CHIP AS BITS(RP23500))
_ _ _ _ _ _ |
| 2 | 0x000001ff | 1 | 0xff(size_type == 1, item_type_ == PICOBIN BLOCK ITEM 2BS LAST)
_ _ _ _ |
|  |  | 2 | 0x0001 (size) |
|  |  | 1 | 0x00 (pad) |
| 3 | 0x00000000 | 4 | Relative pointer to next block in block loop - 0x00000000 means link to self (a loop
containing just this block) |
| 4 | 0xab123579 | 4 | PICOBIN BLOCK MARKER END
_ _ _ |

The LE Value column indicates a 32-bit little-endian value that should appear verbatim in your program image.

Since the above block does not specify an explicit entry point, the bootrom will assume the binary starts with a Cortex-M

vector table, and enter via the reset handler and initial stack pointer specified in that table (offsets +4 and +0 bytes into

the table). An explicit vector table pointer can be provided by a PICOBIN_BLOCK_ITEM_1BS_VECTOR_TABLE item, or the entry

point can be specified directly by a PICOBIN_BLOCK_ITEM_1BS_ENTRY_POINT item.

5.9.5.2. Minimum RISC-V IMAGE_DEF

The minimum valid IMAGE_DEF is the following 20-byte sequence:

5.9. Metadata block details
428
