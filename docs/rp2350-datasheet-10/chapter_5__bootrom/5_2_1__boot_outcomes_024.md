---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2.1. Boot outcomes
pages: 366-367
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 5.2.1. Boot outcomes

5.2.1. Boot outcomes

The bootrom decides the boot outcome based on the following system state:

• The contents of the attached QSPI memory device on chip select 0, if any
• The contents of POWMAN registers BOOT0 through BOOT3
• The contents of watchdog registers SCRATCH4 through SCRATCH7
• The contents of OTP, particularly CRIT1, BOOT_FLAGS0 and BOOT_FLAGS1
• The QSPI CSn pin being driven low externally (to select BOOTSEL)
• The QSPI SD1 pin being driven high externally (to select UART boot in BOOTSEL mode)

Based on these, the outcome of the boot sequence may be to:

• Call code through a vector specified in SCRATCH or BOOT registers prior to the most recent reboot, for example, into

code retained in RAM following a power-up from a low-power state.
• Run an image from external flash.

◦This can happen in one of two ways: either the image runs in-place directly from external flash, or the image is

loaded into RAM during the boot sequence.

◦In-package flash on RP2354 is external for boot purposes. It’s a separate silicon die, and the RP2350 die

doesn’t implicitly trust it.
• Run an image preloaded into SRAM (distinct from the vector case).
• Load and run an image from OTP into SRAM.
• Enter the USB bootloader.
• Enter the UART bootloader.
• Perform a one-shot operation requested via the reboot() API, such as a flash update boot. This may be requested

5.2. Processor-controlled boot sequence
365

RP2350 Datasheet

by the user, or by the UART or UF2 bootloaders.
• Refuse to boot, due to lack of suitable images and the UART and USB bootloaders being disabled via OTP.

This section makes no distinction between the different types of flash boot (flash image boot, flash partition boot and

flash partition-table-in-image boot). Likewise, it does not distinguish these types from packaged binaries, which are

loaded into RAM at boot time, because these are just flash binaries with a special load map. This section just describes

the sequence of decisions the bootrom makes to decide which medium to boot from.
