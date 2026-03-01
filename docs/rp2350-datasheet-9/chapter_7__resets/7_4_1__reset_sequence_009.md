---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.4.1. Reset sequence
pages: 499-500
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 7.4.1. Reset sequence

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

RP2350 Datasheet

Following a watchdog reset trigger, the PSM restarts from a point selected by the PSM WDSEL register.
