---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.4. System resets (Power-on State Machine)
pages: 498-499
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 7.4. System resets (Power-on State Machine)

7.4. System resets (Power-on State Machine)

7.4. System resets (Power-on State Machine)
497

RP2350 Datasheet

![Page 499 figure](images/fig_p0499.png)

*Figure 27. Power-on State Machine Sequence Clocks Boot RAM SRAM 0–9 XIP Cache SIO Access Control Processors*

System Resets apply to components essential to processor operation. System components have interdependencies,

therefore their resets are de-asserted in sequence by the Power-on State Machine (PSM). Each stage of the sequencer

outputs a reset done signal when complete, rst_done, which releases the reset input to the next stage. A partial

sequence runs after a write to the FRCE_OFF register or a watchdog timeout. Note that the FRCE_ON register is intended for

internal use only and is disabled in production devices.

The Power-on State Machine sequences system-level reset release following a power-up of the switched core power

domain. It is distinct from the power manager (POWMAN) which controls power domain switching, see Section 6.2,

“Power management”.
