---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2. Processor-controlled boot sequence
pages: 366-366
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 5.2. Processor-controlled boot sequence

5.2. Processor-controlled boot sequence

The bootrom contains the first instructions the processors execute following a reset. Both processors enter the

bootrom at the same time, and in the same location, but the boot sequence runs mostly on core 0.

Core 1 redirects very early in the boot sequence to a low-power state where it waits to be launched, after boot, by user

software on core 0. If core 1 is unused, it remains in this low-power state.

Source Code Reference

The sequence described in this section is implemented on Arm by the source files arm8_bootrom_rt0.S

and varm_boot_path.c in the bootrom source code repository. RISC-V cores instead begin from

riscv_bootrom_rt0.S, but share the boot path implementation with Arm.
