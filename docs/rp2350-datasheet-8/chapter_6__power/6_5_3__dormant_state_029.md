---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.5.3. DORMANT state
pages: 490-491
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 6.5.3. DORMANT state

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

RP2350 Datasheet

If you halt the crystal oscillator (XOSC), you must also halt the PLLs to prevent them losing lock when their input

reference clock stops. The PLL VCO may behave erratically when the frequency reference is lost, such as increasing to

a very high frequency. Reconfigure and re-enable the PLLs after the XOSC starts again. Do not attempt to run clocks

from the PLLs while the XOSC is stopped.

The DORMANT state is entered by writing a keyword to the DORMANT register in whichever oscillator is active: ring

oscillator (Section 8.3) or crystal oscillator (Section 8.2). If both are active, the one providing the processor clock must

be stopped last because it will stop software from executing.

6.5.3.1. Waking from the DORMANT state

The system exits the DORMANT state on any of the following events:

• an alarm from the AON Timer which causes TIMER.ALARM to assert
• the assertion of an interrupt from GPIO Bank 0 to the DORMANT_WAKE interrupt destination
• the assertion of an interrupt from GPIO Bank 1 to the DORMANT_WAKE interrupt destination

When waking from the AON Timer you do not have to enable the IRQ output from POWMAN. It is sufficient for the timer

to fire, without being mapped to an interrupt output. Any AON Timer alarm comparison event which causes

TIMER.ALARM to assert causes the system to exit the DORMANT state. It is the actual alarm event which causes the

exit, not the TIMER.ALARM status; if you enter the DORMANT state with the TIMER.ALARM status set to 1, but the timer

alarm comparison logic disabled by TIMER.ALARM_ENAB, you will not exit the DORMANT state.

The GPIO Bank registers have interrupt enable registers for interrupts targeting the DORMANT mode wake logic, such

as DORMANT_WAKE_INTE0. These are identical to the interrupt enable registers for interrupts targeting the processors,

such as PROC0_INTE0.

Waking from the DORMANT state restarts the oscillator which was disabled by entry to the DORMANT state. It does not

restart any other oscillators, or change any system-level clock configuration.
