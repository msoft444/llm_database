---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.10. Packaged binaries
pages: 359-359
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.1.10. Packaged binaries

5.1.10. Packaged binaries

As described in Section 5.1.9, signed binaries in flash on a secured RP2350 are commonly loaded from flash into RAM,

go through signature verification in RAM, and then execute from the verified version in RAM.

A packaged binary is a binary stored in flash that runs entirely from RAM. The binary is likely compiled to run from RAM

as a RAM-only binary (unfortunately named no_flash in SDK parlance), but subsequently post-processed for flash

5.1. Bootrom concepts
358
