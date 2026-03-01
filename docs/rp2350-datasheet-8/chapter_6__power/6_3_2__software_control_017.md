---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.3.2. Software control
pages: 450-450
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 6.3.2. Software control

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
