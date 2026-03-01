---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.3.3. Power Manager control
pages: 450-450
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 6.3.3. Power Manager control

![Page 450 figure](images/fig_p0450.png)

RP2350 Datasheet

regulator. If the on-chip regulator is supplying DVDD, entering high-impedance mode causes a reset event, returning the

on-chip regulator to Normal mode.

6.3.2. Software control

WARNING

The regulator can’t be relocked after it’s been unlocked. Avoid accidental writes to the VREG register.

The regulator can be directly controlled by software, but must first be unlocked by writing a 1 to the UNLOCK field in the

VREG_CTRL register. Once unlocked, the regulator can be controlled via the VREG register.

The regulator’s operating mode defaults to Normal, at initial power up or after a reset event, but can be switched to high

impedance by writing a 1 to the VREG register’s HIZ field. The regulator’s output voltage can be set by writing to the

register’s VSEL field, see the VREG register description for details on available settings. To prevent accidental over-

voltage, the output voltage is limited to 1.3 V unless the DISABLE_VOLTAGE_LIMIT field in the VREG_CTRL is set. The output

voltage defaults to 1.1 V at initial power-on or after a reset event.

The UPDATE_IN_PROGRESS field in the VREG register is set while the regulator’s operating mode or output voltage are being

updated. When UPDATE_IN_PROGRESS is set, writes to the register are ignored.

It isn’t possible to place the regulator in low-power mode under software control because the load current will exceed

1mA when software is running.

CAUTION

The regulator’s output voltage can be varied between 0.55 V and 3.3 V, but RP2350 might not operate reliably with

its digital core supply (DVDD) at a voltage other than 1.1 V.

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
