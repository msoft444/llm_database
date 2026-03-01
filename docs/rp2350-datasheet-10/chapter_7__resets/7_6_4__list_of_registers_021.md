---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.6.4. List of registers
pages: 513-513
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 7.6.4. List of registers

7.6.4. List of registers

The chip-level reset subsystem shares a register address space with other power management subsystems in the

always-on domain. The address space is referred to as POWMAN elsewhere in this document. A complete list of POWMAN

registers is provided in Section 6.4, “Power management (POWMAN) registers”, but information on registers associated

with the brownout detector are repeated here.

The POWMAN registers start at a base address of 0x40100000 (defined as POWMAN_BASE in SDK).

• BOD_CTRL
• BOD
• BOD_LP_ENTRY
• BOD_LP_EXIT

7.6. Power-on resets and brownout detection
512
