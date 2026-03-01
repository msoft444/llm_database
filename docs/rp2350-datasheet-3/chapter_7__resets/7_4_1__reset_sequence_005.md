---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.4.1. Reset sequence
pages: 499-499
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 7.4.1. Reset sequence

![Page 499 figure](images/fig_p0499.png)

RP2350 Datasheet

Figure 27. Power-on

State Machine

Chip Level Reset 

Sequence

Released

Ring Oscillator

OTP
Crystal Oscillator

Bus Fabric
Boot ROM
PSM Ready

Clocks
Boot RAM

SRAM 0–9
XIP Cache
SIO
Access Control
Processors

System Resets apply to components essential to processor operation. System components have interdependencies,

therefore their resets are de-asserted in sequence by the Power-on State Machine (PSM). Each stage of the sequencer

outputs a reset done signal when complete, rst_done, which releases the reset input to the next stage. A partial

sequence runs after a write to the FRCE_OFF register or a watchdog timeout. Note that the FRCE_ON register is intended for

internal use only and is disabled in production devices.

The Power-on State Machine sequences system-level reset release following a power-up of the switched core power

domain. It is distinct from the power manager (POWMAN) which controls power domain switching, see Section 6.2,

“Power management”.

7.4.1. Reset sequence

Following a chip-level reset, the Power-on State Machine (PSM):

1. Removes cold reset to processors.

2. Takes OTP out of reset. OTP reads any content required to boot and asserts rst_done.

3. Starts the Ring Oscillator. Asserts rst_done once the oscillator output is stable.

4. Removes Crystal Oscillator (XOSC) controller reset. The XOSC does not start yet, so rst_done is asserted

immediately.

5. Deasserts the master subsystem reset, but does not remove individual subsystem resets.

6. Starts the clk_ref and clk_sys clock generators. In the initial configuration, clk_ref runs from the ring oscillator with

no divider and clk_sys runs from clk_ref.

7. The PSM confirms the clocks are active.

8. Removes Bus Fabric reset and initialises logic.

9. Removes various memory controllers' resets and initialises logic.

10. Removes Single-cycle IO subsystem (SIO) reset and initialises logic.

11. Removes Access Controller reset and initialises logic.

12. Deasserts Processor Complex reset. Both core 0 and core 1 start executing the boot code from ROM. The boot

code reads the core id and core 1 sleeps, leaving core 0 to continue bootrom execution.

7.4. System resets (Power-on State Machine)
498
