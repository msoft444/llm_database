---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.1.1. Changes from RP2040
pages: 878-878
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 11.1.1. Changes from RP2040

11.1.1. Changes from RP2040

RP2350 adds the following new registers and controls:

• DBG_CFGINFO.VERSION indicates the PIO version, to allow PIO feature detection at runtime.

◦This 4-bit field was reserved-0 on RP2040 (indicating version 0), and reads as 1 on RP2350.
• GPIOBASE adds support for more than 32 GPIOs per PIO block.

◦Each PIO block is still limited to 32 GPIOs at a time, but GPIOBASE selects which 32.
• CTRL.NEXT_PIO_MASK and CTRL.PREV_PIO_MASK apply some CTRL register operations to state machines in

neighbouring PIO blocks simultaneously.

◦CTRL.NEXTPREV_SM_DISABLE stops PIO state machines in multiple PIO blocks simultaneously.

◦CTRL.NEXTPREV_SM_ENABLE starts PIO state machines in multiple PIO blocks simultaneously.

◦CTRL.NEXTPREV_CLKDIV_RESTART synchronises the clock dividers of PIO state machines in multiple PIO

blocks
• SM0_SHIFTCTRL.IN_COUNT masks unneeded IN-mapped pins to zero.

◦This is useful for MOV x, PINS instructions, which previously always returned a full rotated 32-bit value.
• IRQ0_INTE and IRQ1_INTE now expose all eight SM IRQ flags to system-level interrupts (not just the lower four).
• Registers starting from RXF0_PUTGET0 expose each RX FIFO’s internal storage registers for random read or write

access from the system,

◦The new FJOIN_RX_PUT FIFO join mode enables random writes from the state machine, and random reads from

the system (for implementing status registers).

◦The new FJOIN_RX_GET FIFO join mode enables random reads from the state machine, and random writes from

the system (for implementing control registers).

◦Setting both FJOIN_RX_PUT and FJOIN_RX_GET enables random read and write access from the state machine, but

disables system access.

11.1. Overview
877
