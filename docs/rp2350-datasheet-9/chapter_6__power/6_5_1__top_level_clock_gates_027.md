---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.5.1. Top-level clock gates
pages: 490-490
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 6.5.1. Top-level clock gates

RP2350 Datasheet

6.5.1. Top-level clock gates

Each clock domain (for example, the system clock) may drive a large number of distinct hardware blocks, not all of

which might be required at once. To avoid unnecessary power dissipation, each individual endpoint of each clock (for

example, the UART system clock input) may be disabled at any time.

Enabling and disabling a clock gate is glitch-free. If a peripheral clock is temporarily disabled, and subsequently re-

enabled, the peripheral will be in the same state as prior to the clock being disabled. No reset or reinitialisation should

be required.

Clock gates are controlled by two sets of registers: the WAKE_ENx registers (starting at WAKE_EN0) and SLEEP_ENx registers

(starting at SLEEP_EN0). These two sets of registers are identical at the bit level, each possessing a flag to control each

clock endpoint. The WAKE_EN registers specify which clocks are enabled whilst the system is awake, and the SLEEP_ENx

registers select which clocks are enabled while the processor is in the SLEEP state (Section 6.5.2).

The two processors do not have externally-controllable clock gates. Instead, the processors gate the clocks of their

subsystems autonomously, based on execution of WFI/WFE instructions, and external Event and IRQ signals.
