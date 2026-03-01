---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.3.9. List of registers
pages: 458-458
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 6.3.9. List of registers

![Page 458 figure](images/fig_p0458.png)

6.3.9. List of registers

The voltage regulator shares a register address space with other power management subsystems in the always-on

domain. This address space is referred to as POWMAN elsewhere in this document, and a complete list of POWMAN registers is

provided in Section 6.4. For reference information on POWMAN registers associated with the voltage regulator is repeated

here.

The POWMAN registers start at a base address of 0x40100000 (defined as POWMAN_BASE in the SDK).

• VREG_CTRL
• VREG_STS
• VREG
• VREG_LP_ENTRY
• VREG_LP_EXIT
