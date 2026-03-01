---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.6.3. Supply monitor
pages: 513-513
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 7.6.3. Supply monitor

7.6.3. Supply monitor

The power-on and brownout reset blocks are powered by the core voltage regulator’s analogue supply (VREG_AVDD). The

blocks are initialised when power is first applied, but may not be reliably re-initialised if power is removed and then

reapplied before VREG_AVDD has dropped to a sufficiently low level. To prevent this happening, VREG_AVDD is monitored and

the power-on reset block is re-initialised if it drops below the VREG_AVDD activation threshold (VREG_AVDDTH.ACTIVE).

VREG_AVDDTH.ACTIVE is fixed at a nominal 1.1V, which should result in a threshold between 0.87V and 1.26V. This

threshold does not represent a safe operating voltage. Instead, it represents the voltage that VREG_AVD must drop below

to reliably re-initialise the power-on reset block. For safe operation, VREG_AVDD must be at a nominal voltage of 3.3V. See

Table 1441, “Power Supply Specifications”.

7.6.3.1. Detailed specifications

| Parameter | Description | Min | Typ | Max | Units |
| --- | --- | --- | --- | --- | --- |
| VREG_VIN TH.ACTIVE | VREG VIN activation _ threshold | 0.87 | 1.1 | 1.26 | V |

*Table 540. Voltage Regulator Input Supply Monitor Parameters*
