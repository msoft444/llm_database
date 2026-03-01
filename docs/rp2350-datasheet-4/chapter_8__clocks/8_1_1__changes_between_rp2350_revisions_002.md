---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.1.1. Changes between RP2350 revisions
pages: 515-515
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 8.1.1. Changes between RP2350 revisions

8.1.1. Changes between RP2350 revisions

RP2350 A3 changes the reset values of:

• CLK_SYS_CTRL.SRC from 0 to 1 (select AUX source).
• CLK_SYS_CTRL.AUXSRC from 0 to 2 (select ROSC as AUX source).

See Hardware changes for information about related changes made to the ROSC configuration at reset. See Bootrom

changes for related changes made in the A3 boot ROM.
