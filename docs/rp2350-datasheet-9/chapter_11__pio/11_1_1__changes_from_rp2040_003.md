---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.1.1. Changes from RP2040
pages: 878-879
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
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

RP2350 Datasheet

RP2350 adds the following new instruction features:

• Adds PINCTRL_JMP_PIN as a source for the WAIT instruction, plus an offset in the range 0-3.

◦This gives WAIT pin arguments a per-SM mapping that is independent of the IN-mapped pins.
• Adds PINDIRS as a destination for MOV.

◦This allows changing the direction of all OUT-mapped pins with a single instruction: MOV PINDIRS, NULL or MOV

PINDIRS, ~NULL
• Adds SM IRQ flags as a source for MOV x, STATUS

◦This allows branching (as well as blocking) on the assertion of SM IRQ flags.
• Extends IRQ instruction encoding to allow state machines to set, clear and observe IRQ flags from different PIO

blocks.

◦There is no delay penalty for cross-PIO IRQ flags: an IRQ on one state machine is observable to all state

machines on the next cycle.
• Adds the FJOIN_RX_GET FIFO mode.

◦A new MOV encoding reads any of the four RX FIFO storage registers into OSR.

◦This instruction permits random reads of the four FIFO entries, indexed either by instruction bits or the Y

scratch register.
• Adds the FJOIN_RX_PUT FIFO mode.

◦A new MOV encoding writes the ISR into any of the four RX FIFO storage registers.

◦The registers are indexed either by instruction bits or the Y scratch register.

RP2350 adds the following security features:

• Limits Non-secure PIOs (set to via ACCESSCTRL) to observation of only Non-secure GPIOs. Attempting to read a

Secure GPIO returns a 0.
• Disables cross-PIO functionality (IRQs, CTRL_NEXTPREV operations) between Non-secure PIO blocks (those which

permit Non-secure access according to ACCESSCTRL) and Secure-only blocks (those which do not).

RP2350 includes the following general improvements:

• Increased the number of PIO blocks from two to three (8 → 12 state machines).
• Improved GPIO input/output delay and skew.
• Reduced DMA request (DREQ) latency by one cycle vs RP2040.
