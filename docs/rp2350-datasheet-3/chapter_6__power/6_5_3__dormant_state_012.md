---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.5.3. DORMANT state
pages: 490-490
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 6.5.3. DORMANT state

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

6.5.2. SLEEP state

RP2350 enters the SLEEP state when all of the following are true:

• Both processors are asleep (e.g. in a WFE or WFI instruction)
• The system DMA has no outstanding transfers on any channel

RP2350 exits the SLEEP state when either processor is awoken by an interrupt.

When in the SLEEP state, the top-level clock gates are masked by the SLEEP_ENx registers (starting at SLEEP_EN0), rather

than the WAKE_ENx registers (starting at WAKE_EN0). This permits more aggressive pruning of the clock tree when the

processors are asleep.

NOTE

Though it is possible for a clock to be enabled during SLEEP and disabled outside of SLEEP, this is generally not

useful.

For example, if the system is sleeping until a character interrupt from a UART, the entire system except for the UART

can be clock-gated (SLEEP_ENx = all-zeroes except for CLK_SYS_UART0 and CLK_PERI_UART0). This includes system

infrastructure such as the bus fabric.

When the UART asserts its interrupt and wakes a processor, RP2350 leaves SLEEP mode and switches back to the

WAKE_ENx clock mask. At the minimum, this should include the bus fabric and the memory devices containing the

processor’s stack and interrupt vectors.

A system-level clock request handshake holds the processors off the bus until the clocks are re-enabled.

6.5.3. DORMANT state

The DORMANT state is a true zero-dynamic-power sleep state, where all clocks (and all oscillators) are disabled. The

system can awake from the DORMANT state upon a GPIO event (high/low level or rising/falling edge), or an AON Timer

alarm: this restarts one of the oscillators (either ring oscillator or crystal oscillator) and ungates the oscillator output

after it is stable. System state is retained, so code execution resumes immediately upon leaving the DORMANT state.

If relying on the AON Timer (Section 12.10) to wake from the DORMANT state, the AON Timer must run from the LPOSC

or an external clock source. The AON Timer accepts clock frequencies as low as 1Hz.

DORMANT does not halt PLLs. To avoid unnecessary power dissipation, software should power down PLLs before

entering the DORMANT state, and power up and reconfigure the PLLs again after exiting.

6.5. Power reduction strategies
489
