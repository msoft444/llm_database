---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.1.6. Core voltage regulator input supply (VREG_VIN)
pages: 443-443
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 6.1.6. Core voltage regulator input supply (VREG_VIN)

6.1.6. Core voltage regulator input supply (VREG_VIN)

VREG_VIN is the input supply for the on-chip core voltage regulator, and should be in the range 2.7 V to 5.5 V. To reduce

the number of external power supplies, VREG_VIN can use the same power source as the voltage regulator analogue

supply (VREG_AVDD), or digital IO supply (IOVDD). Though care should be taken to minimise the noise on VREG_AVDD.

A 4.7μF capacitor should be connected between VREG_VIN and ground close to the chip’s VREG_VIN pin.

For more details on the on-chip voltage regulator see Section 6.3.
