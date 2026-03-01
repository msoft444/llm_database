---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.6. USB PICOBOOT interface
pages: 404-404
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 5.6. USB PICOBOOT interface

5.6. USB PICOBOOT interface

The PICOBOOT interface is a low level USB protocol for interacting with the RP2350 while it is in BOOTSEL mode. This

interface may be used concurrently with the USB Mass Storage Interface.

It provides for flexible reading from and writing to RAM or flash, rebooting, executing code on the device and a handful

of other management functions.

Constants and structures related to the interface can be found in the SDK header picoboot.h in the SDK
