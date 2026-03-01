---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: Chapter 5. Bootrom
pages: 354-355
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# Chapter 5. Bootrom

RP2350 Datasheet

Chapter 5. Bootrom

Each RP2350 device contains 32 kB of mask ROM: a physically immutable memory resource described in Section 4.1.

The RP2350 bootrom is the binary image etched into this ROM that contains the first instructions executed at reset.

The bootrom concepts section (Section 5.1) covers the following topics, which are necessary background for

understanding the bootrom features and their implementation:

• Partition tables and their associated flash permissions
• Bootable images, and the block loops that store their metadata
• Versioning for images and partition tables, and A/B versions to support double-buffered upgrades
• Hashing and signing to support secure boot with public key fingerprint in OTP (see also Section 10.1.1 in the

security chapter)
• Load maps for bootable images, and packaged binaries that the bootrom loads from flash into RAM according to

the image’s load map
• Anti-rollback protection to revoke older, compromised versions of software
• Three forms of flash boot:

◦Flash image boot, with a single binary image written directly into flash

◦Flash partition boot, with the boot image selected from the partition table

◦Partition-table-in-image boot, where the boot image is not contained in a partition table, but still embeds a

partition table data structure to divide the flash address space
• Boot slots for A/B versions of partition tables
• Flash update boot, a special one-time boot mode that enables version downgrades following an image download
• Try before you buy support for phased upgrades with image self-test
• Address translation for flash images, which provides a consistent runtime address to images regardless of

physical storage location
• Automatic architecture switch when attempting to run a RISC-V binary on Arm, or vice versa
• Targeting UF2 downloads to different flash partitions based on their permissions and the UF2 family ID

Besides features mentioned as concepts above, the RP2350 bootrom implements:

• The core 0 initial boot sequence (Section 5.2)
• The core 1 low-power wait and launch protocol (Section 5.3)
• Runtime APIs (Section 5.4) exported through the ROM symbol table, such as flash and OTP programming
• A subset of runtime APIs available to Non-secure code, with permission for each API entry point individually

configured by Secure code
• A USB MSC class-compliant bootloader with UF2 support for downloading code/data to flash or RAM (Section 5.5),

including support for versioning and A/B partitions
• The USB PICOBOOT interface for advanced operations like OTP programming (Section 5.6) and to support picotool

or other host side tools
• Support for white-labelling all USB exposed information/identifiers (Section 5.7)
• A UART bootloader providing a minimal shell to load an SRAM binary from a host microcontroller (Section 5.8)

You should read the bootrom concepts section before diving into the features in the list above. RP2350 adds a

considerable amount of new functionality compared to the RP2040 bootrom. If you are in a terrible hurry, Section 5.9.5

covers the absolute minimum requirements for a binary to be bootable on RP2350 when secure boot is not enabled.

Chapter 5. Bootrom
353

RP2350 Datasheet

Bootrom source code

All source files for the RP2350 bootrom are available under the terms of the 3-clause BSD licence:

github.com/raspberrypi/pico-bootrom-rp2350
