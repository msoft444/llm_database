---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.3.3. Power Manager control
pages: 450-451
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 6.3.3. Power Manager control

6.3.3. Power Manager control

The regulator’s operating mode and output voltage can also be controlled by the Power Manager. Power Manager

control is typically used when the chip enters or exits a low-power (P1.x) state, when software might not be running.

In addition to normal and high-impedance modes, Power Manager control allows the regulator to be placed in low-

power mode. By default, the regulator switches to low-power mode when entering a low-power (P1.x) state, and returns

to Normal mode when returning to a normal (P0.x) state.

The operating mode and output voltage in the low-power state are set by the values in the VREG_LP_ENTRY register.

And the operating mode and output voltage to be used when the chip has returned to a normal state are set by values in

the VREG_LP_EXIT register. The registers contain an additional MODE field that allows low-power mode to be selected.

The values in the registers must be written by software before requesting a transition to a low-power state because

software won’t be running during or after the transition. The actual transitions to and from the low-power state are

handled by the Power Manager. Once the chip has returned to a normal state, software can be run and the regulator

controlled directly. The values in the VREG register reflect the regulator’s current operating mode and output voltage

once the chip has returned to a normal state.

6.3. Core voltage regulator
449

RP2350 Datasheet

CAUTION

Low-power mode should only be used when the regulator is providing the chip’s digital core supply (DVDD) because

the regulator’s low-power output is connected to DVDD on chip.
