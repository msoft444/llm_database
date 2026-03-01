---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.5.3. DORMANT state
pages: 490-490
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
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
