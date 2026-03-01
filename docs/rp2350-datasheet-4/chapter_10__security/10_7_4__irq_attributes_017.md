---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.7.4. IRQ attributes
pages: 869-869
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 10.7.4. IRQ attributes

10.7.4. IRQ attributes

Each of the four shared DMA interrupt lines (IRQs) has a configurable security level. The IRQ’s security level is

compared with channel security levels, and with the bus privilege of accesses to the DMA’s interrupt control registers, to

determine:

• Whether a bus access is permitted to read/write the INTE/INTF/INTS registers for this IRQ
• Whether a given channel will be visible in this IRQ’s INTS register (and therefore whether that channel will cause

10.7. DMA
868
