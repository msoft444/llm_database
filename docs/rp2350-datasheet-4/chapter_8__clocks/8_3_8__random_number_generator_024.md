---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.3.8. Random number generator
pages: 565-565
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 8.3.8. Random number generator

RP2350 Datasheet

8.3.8. Random number generator

When the system clocks are running from the XOSC, you can use the ROSC to generate random numbers. Enable the

ROSC and read the RANDOMBIT register to get a 1-bit random number; to get an n-bit value, read it n times. This does not

meet the requirements of randomness for security systems because it can be compromised, but it may be useful in less

critical applications. If the cores are running from the ROSC, the value will not be random because the timing of the

register read will be correlated to the phase of the ROSC.
