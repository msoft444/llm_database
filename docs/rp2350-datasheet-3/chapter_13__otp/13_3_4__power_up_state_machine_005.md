---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.3.4. Power-up state machine
pages: 1273-1273
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 13.3.4. Power-up state machine

RP2350 Datasheet

◦Allows arbitrary OTP operations, including programming

◦Only accessible to Secure reads and writes
• Provides control registers for Raspberry Pi hardware

◦Registers have different accessibility according to Secure/Non-secure and read/write

◦Software lock registers are always readable by both security domains

Hardware configuration data read from OTP during the power-up sequence drives system-level control signals, e.g.

disabling CoreSight APs. This is described in more detail in (Section 13.3.4).

A single system-level interrupt output (IRQ) generates interrupts for the following sources:

• Secure read failed due to locks
• Non-secure read failed due to locks
• Write failed due to locks
• SBPI FLAG, used by the PMC to signal completion
• Data port access when DCTRL is set error

◦USR.DCTRL tells the SNPS AP whether the SBPI bridge or data bridge can access the memory array; this help

debugging SW if a data access is attempted whilst the DAP is inaccessible

Any failed access also returns a bus fault (PSLVERR). To determine whether an OTP address is accessible, query the lock

tables.

Non-secure code cannot access the interrupt status registers.

13.3.3. OTP boot oscillator

The OTP startup sequence (Section 13.3.4) runs from a local ring oscillator, dedicated to the OTP subsystem. This is

separate from the system ring oscillator (the ROSC) which provides the system clock to run the processors during boot.

• The OTP boot oscillator is the only clock used by the OTP power-up state machine
• The OTP boot oscillator dynamically randomises its own frequency controls, to deliberately add jitter to the clock
• The OTP boot oscillator stops when the PSM completes, and does not start again until the OTP resets
• The OTP clock automatically switches to clk_ref when the OTP boot oscillator stops

The boot oscillator has a nominal frequency of 12MHz. It provides the clock for reading out hardware configuration

from OTP, including the critical flags (Section 13.4) which configure hardware security features such as debug disable

and the glitch detectors.

Keeping this oscillator local to the OTP hardware subsystem reduces the power signature of the clock itself, due to the

lower switched clock capacitance. Along with the random jitter of the frequency controls, this helps frustrate attempts

to recover OTP access keys and debug keys via power signature analysis attacks, or to disable security features by

timing fault injection against the OTP clock.

Only the OTP boot oscillator enables the ROSC frequency randomisation feature by default: for later operations using

the system ROSC (Section 8.3), you must explicitly enable this feature on that oscillator, by programming the ROSC

control registers. The crystal oscillator (XOSC) does not support frequency randomisation.

13.3.4. Power-up state machine

The OTP is the second item in the switched core domain’s Power-On State Machine (Section 7.4), after the processor

cold reset. OTP does not release its rst_done, or enable any debug interface (including the factory test JTAG described in

Section 10.10), until the OTP PSM reads out OTP-resident hardware configuration. The rst_done output to the system

13.3. Background: OTP hardware architecture
1272
