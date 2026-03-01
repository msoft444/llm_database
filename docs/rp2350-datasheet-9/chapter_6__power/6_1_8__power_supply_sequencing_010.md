---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.1.8. Power supply sequencing
pages: 444-444
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 6.1.8. Power supply sequencing

RP2350 Datasheet

6.1.8. Power supply sequencing

With the exception of the two voltage regulator supplies (VREG_VIN and VREG_AVDD), which should be powered up together,

RP2350’s power supplies may be powered up or down in any order. However, small transient currents may flow in the

ADC supply (ADC_AVDD) if it is powered up before, or powered down after, the digital core supply (DVDD). This will not

damage the chip, but can be avoided by powering up DVDD before or at the same time as ADC_AVDD, and powering down

DVDD after or at the same time as ADC_AVDD. In the most common power supply scheme, where the chip is powered from

a single 3.3 V supply, DVDD will be powered up shortly after ADC_AVDD due to the startup time of the on-chip voltage

regulator. This is acceptable behaviour.
