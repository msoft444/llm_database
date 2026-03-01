---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.20. Automatic architecture switching
pages: 365-366
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 5.1.20. Automatic architecture switching

5.1.20. Automatic architecture switching

If the bootrom encounters a valid and correctly signed IMAGE_DEF for the non-current architecture (RISC-V when booted in

Arm mode, or Arm when booted in RISC-V), it performs an automatic architecture switch. The bootrom initiates a reboot

into the correct architecture for the binary it discovered, which then boots successfully on the second attempt.

Information passed in watchdog scratch registers (such as a RAM image boot type) is retained, so that the second boot

makes the same decisions as the first, and arrives at the same preferred image to boot.

This happens only when:

• The architecture to be switched to is available according to OTP critical flags
• The architecture switch feature is not disabled by the BOOT_FLAGS0.DISABLE_AUTO_SWITCH_ARCH flag
• The bootrom found no valid binary for the current architecture before finding one for the other architecture

5.1. Bootrom concepts
364

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
