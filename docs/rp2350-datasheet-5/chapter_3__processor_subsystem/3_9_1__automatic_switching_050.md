---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.9.1. Automatic switching
pages: 336-337
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 3.9.1. Automatic switching

![Page 336 figure](images/fig_p0336.png)

3.9.1. Automatic switching

RP2350 binaries contain a binary marker recognised by the bootrom. This marker:

• contains additional information such as the binary’s entry point and the intended architecture: Arm, RISC-V, or both
• helps detect when a flash device is connected
• helps verify that the flash device was accessed using the correct SPI parameters

When booting with core 0 in Arm architecture mode, upon detecting a bootable RISC-V binary, the bootrom

automatically resets both cores and switches them to RISC-V architecture mode. After the reset, the bootrom detects

that the binary and processor architectures match, so the binary launches normally.

Likewise, when booting with core 0 in RISC-V architecture mode, upon detecting a bootable Arm binary, the bootrom

automatically resets both cores and switches them to Arm architecture mode.

3.9. Arm/RISC-V architecture switching
335

RP2350 Datasheet

As a result, the USB bootloader, which runs on both Arm and RISC-V, can accept a UF2 image download for either

architecture, and automatically boot it using the correct processors.
