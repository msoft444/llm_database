---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.1.3. Digital core supply (DVDD)
pages: 442-443
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 6.1.3. Digital core supply (DVDD)

6.1.3. Digital core supply (DVDD)

The chip’s core digital logic is powered by DVDD, which should be at a nominal 1.1 V. A dedicated on-chip core voltage

regulator allows DVDD to be generated from a 2.7 V to 5.5 V input supply. See Section 6.3 for details. Alternatively, DVDD

can be supplied directly from an off-chip power source.

If the on-chip core voltage regulator is used, the two DVDD pins closest to the regulator should be decoupled with a 100nF

capacitor close to the pins. The DVDD pin furthest from the regulator should be decoupled with a 4.7μF capacitor close to

6.1. Power supplies
441

RP2350 Datasheet

the pin.
