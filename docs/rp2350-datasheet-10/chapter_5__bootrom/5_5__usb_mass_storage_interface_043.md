---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.5. USB mass storage interface
pages: 400-400
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 5.5. USB mass storage interface

5.5. USB mass storage interface

The bootrom provides a standard USB bootloader that makes a writeable drive available for copying code to the RP2350

using UF2 files (see Section 5.5.2).

A suitable UF2 file copied to the drive is downloaded and written to flash or RAM, and the device is automatically

rebooted, making it trivial to download and run code on the RP2350 using only a USB connection.
