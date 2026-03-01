---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.4. Image definitions
pages: 356-357
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 5.1.4. Image definitions

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

RP2350 Datasheet

For details about the IMAGE_DEF format itself, see Section 5.9.3.

For a description of the minimum requirements for a bootable image, see Section 5.9.5.
