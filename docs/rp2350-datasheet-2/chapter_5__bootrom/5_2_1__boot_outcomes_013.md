---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2.1. Boot outcomes
pages: 366-366
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 5.2.1. Boot outcomes

![Page 366 figure](images/fig_p0366.png)

RP2350 Datasheet


TIP

When storing executable images for both architectures in flash, it’s usually preferable to boot an image for the

current architecture. To do this, keep the images in different partitions, marking the partition for Arm as ignored

during boot under RISC-V and vice versa. This avoids always picking the image in the first partition and auto-

switching to run it under the other architecture.

For hardware support details for architecture switching, see Section 3.9.

5.2. Processor-controlled boot sequence

The bootrom contains the first instructions the processors execute following a reset. Both processors enter the

bootrom at the same time, and in the same location, but the boot sequence runs mostly on core 0.

Core 1 redirects very early in the boot sequence to a low-power state where it waits to be launched, after boot, by user

software on core 0. If core 1 is unused, it remains in this low-power state.

Source Code Reference

The sequence described in this section is implemented on Arm by the source files arm8_bootrom_rt0.S

and varm_boot_path.c in the bootrom source code repository. RISC-V cores instead begin from

riscv_bootrom_rt0.S, but share the boot path implementation with Arm.

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
