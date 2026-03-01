---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.1.1. Digital IO supply (IOVDD)
pages: 442-442
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 6.1.1. Digital IO supply (IOVDD)

6.1.1. Digital IO supply (IOVDD)

IOVDD provides the IO supply for the chip’s GPIO, and should be powered at a nominal voltage between 1.8 V and 3.3 V.

The supply voltage sets the external signal level for the digital IO, and should be chosen based on the level required, see

Section 14.9 for details. All GPIOs share the same power supply and operate at the same signal level.

If the digital IO is powered at a nominal 1.8 V, the IO input thresholds should be adjusted by setting the

VOLTAGE_SELECT register to 1. VOLTAGE_SELECT is set to 0 by default, which results in input thresholds that are valid

for a nominal IO voltage between 2.5 V and 3.3 V. See Chapter 9 for details.

CAUTION

Powering the IOVDD at 1.8 V with input thresholds set for a 2.5 V to 3.3 V supply is a safe operating mode, but will

result in input thresholds that do not meet specification. Powering the IO at voltages greater than 1.8 V with input

thresholds set for a 1.8 V supply may result in damage to the chip.
