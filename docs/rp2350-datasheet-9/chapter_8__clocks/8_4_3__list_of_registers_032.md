---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.4.3. List of registers
pages: 571-571
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 8.4.3. List of registers

8.4.3. List of registers

The low power oscillator shares register address space with other power management subsystems in the always-on

domain. The address space is referred to as POWMAN elsewhere in this document. A complete list of POWMAN

registers is provided in Section 6.4, “Power management (POWMAN) registers”, but information on registers associated

with the low power oscillator is repeated here.

The POWMAN registers start at a base address of 0x40100000 (defined as POWMAN_BASE in SDK).

• LPOSC
• EXT_TIME_REF
• LPOSC_FREQ_KHZ_INT
• LPOSC_FREQ_KHZ_FRAC

8.5. Tick generators
