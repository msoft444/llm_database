---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.9. Load maps
pages: 359-359
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 5.1.9. Load maps

5.1.9. Load maps

A load map describes regions of the binary and what to do with them before the bootrom runs the binary.

The load map supports:

• Copying portions of the binary from flash to RAM (or to the XIP cache)
• Clearing parts of RAM (either .bss clear, or erasing uninitialised memory during secure boot)
• Defining what parts of the binary are included in a hash or signature
• Preventing the flushing of the XIP cache when to keep loaded lines pinned up to the point the binary starts

For full details on the LOAD_MAP item type of IMAGE_DEF blocks, see Section 5.9.3.2.

When booting a signed binary from flash, it is desirable to load the signed data and code into RAM before checking the

signature and subsequently executing it. Otherwise, an adversary could replace the flash device in between the

signature check and execution, subverting the check. For this reason, the load map also serves as a convenient

description of what to include in a hash or signature. The load map itself is covered by the hash or signature, and the

entire metadata block is loaded into RAM before processing, so it is not itself subject to this time-of-check versus time-

of-use concern.
