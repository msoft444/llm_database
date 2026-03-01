---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.1.7. On-chip voltage regulator analogue supply (VREG_AVDD)
pages: 443-443
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 6.1.7. On-chip voltage regulator analogue supply (VREG_AVDD)

6.1.7. On-chip voltage regulator analogue supply (VREG_AVDD)

VREG_AVDD supplies the on chip voltage regulator’s analogue control circuits, and should be powered at a nominal 3.3 V.

To reduce the number of external power supplies, VREG_AVDD can use the same power source as the voltage regulator

input supply (VREG_VIN), or the digital IO supply (IOVDD). Though care should be taken to minimise the noise on VREG_AVDD.

A passive low pass filter may be required, see Section 6.3.7 for details.

NOTE

VREG_AVDD also powers the chip’s power-on reset and brownout detection blocks, so it must be powered even if the

on-chip voltage regulator is not used.

6.1. Power supplies
442
