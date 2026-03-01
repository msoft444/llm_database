---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.19. Address translation
pages: 364-364
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
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
