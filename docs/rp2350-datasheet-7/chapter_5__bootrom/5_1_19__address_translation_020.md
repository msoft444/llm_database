---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.19. Address translation
pages: 364-365
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 5.1.19. Address translation

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

RP2350 Datasheet

As an example, if the booted partition was 6 MB big, starting 1 MB into flash, the registers would be set up as follows:

| Name / Memory Start | Flash Start | Size |
| --- | --- | --- |
| ATRANS0 / 0x10000000 | 0x10100000 | 4 MB |
| ATRANS1 / 0x10400000 | 0x10500000 | 2 MB |
| ATRANS2 / 0x10800000 | - | 0 |
| ATRANS3 / 0x10c00000 | - | 0 |

This maps the physical flash range from 0x10100000-0x10700000 to flash addresses 0x10000000-0x10600000, ensuring the

partition appears at the start of flash at runtime.

In this case, since ATRANS2 and ATRANS3 aren’t needed to map the partition, they could be used by the application to

map another part of flash.

Mapping the start of the partition to a runtime address of 0x10000000 is the default behaviour, but you may choose a

different address, with some restrictions. The bootrom allows for runtime address values of 0x10000000, 0x10400000,

0x10800000, and 0x10c00000 for the beginning of the mapped regions, with the choice specified in the IMAGE_DEF. You must

link your binary to run at the correct, higher base address. This is useful, for example, when an application runs from a

high flash address and remains mapped there while launching a second application at address 0x10000000. You might

use this setup when a Secure image provides services to a Non-secure client image.

This custom address translation is enabled by a negative ROLLING_WINDOW_DELTA value (see Section 5.9.3.5). The above

four runtime addresses translate to a ROLLING_WINDOW_DELTA of 0, -0x400000, -0x800000, or -0xc00000, which are the only

supported non-positive values. The delta indicates how many bytes into the partition the runtime address 0x10000000

appears. A negative delta value indicates that address 0x10000000 appears before the start of the partition; the partition

starts that many bytes higher than 0x10000000.

Positive values are also useful, for example, when prepending data to an already linked image as a post-processing

step. Positive deltas must be multiples of 4 kB. For example, a ROLLING_WINDOW_DELTA of 0x1000 will set up address

translation so that the image data starting at offset 0x1000 within the partition is mapped to 0x10000000 at runtime,

omitting the first 4 kB of the image from the mapped region. The first 4 kB is inaccessible except through the

untranslated XIP window, which defaults to Secure access only.

NOTE

Because address translation within the 0x100000000 → 0x11000000 and 0x11000000 → 0x12000000 windows is independent,

it is only possible to boot from partitions which are entirely contained within the first 16 MB of flash.

This address translation is performed by hardware in the QMI. For more information, see Section 12.14.4.
