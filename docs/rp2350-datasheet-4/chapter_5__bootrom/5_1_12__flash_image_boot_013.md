---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.12. Flash image boot
pages: 360-360
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.1.12. Flash image boot

5.1.12. Flash image boot

RP2350 is designed primarily to run code from a QSPI flash device, either in-package or soldered separately to the

circuit board. Code runs either in-place in flash, or in SRAM after being loaded from flash. Flash boot is the process of

discovering that code and preparing to run it. Flash image boot uses a program binary stored directly in flash rather

than in a flash partition. Flash image boot requires the bootrom to discover a block loop starting within the first 4 kB of

flash which contains a valid IMAGE_DEF (and no PARTITION_TABLE).

5.1. Bootrom concepts
359
