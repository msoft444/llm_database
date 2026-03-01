---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.2.2. Power states
pages: 445-446
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 6.2.2. Power states

6.2.2. Power states

RP2350 can operate in a number of power states, depending on which domains are powered on or off. Power states

have names in the form Pc.m where:

• c indicates the state of the switched core (SWCORE) domain: 0 = on / 1 = off
• m is a 3 bit binary representation of the memory power domains, in the order XIP, SRAM0, SRAM1

P0.m states, where the switched core is powered on, are Normal Operating states. P1.m states, where the switched core is

powered off, are Low Power states

Table 475 shows the available power states.

| Power State | Description | AON | SWCORE | XIP | SRAM0 | SRAM1 |
| --- | --- | --- | --- | --- | --- | --- |
| P0.0 | Normal Operation | on | on | on | on | on |
| P0.1 | Normal Operation (SRAM1 off) | on | on | on | on | off |
| P0.2 | Normal Operation (SRAM0 off) | on | on | on | off | on |
| P0.3 | Normal Operation (SRAM0 & SRAM1 off) | on | on | on | off | off |
| P1.0 | Low Power | on | off | on | on | on |
| P1.1 | Low Power (SRAM1 off) | on | off | on | on | off |
| P1.2 | Low Power (SRAM0 off) | on | off | on | off | on |
| P1.3 | Low Power (SRAM0 & SRAM1 off) | on | off | on | off | off |
| P1.4 | Low Power (XIP off) | on | off | off | on | on |
| P1.5 | Low Power (XIP & SRAM1 off) | on | off | off | on | off |
| P1.6 | Low Power (XIP & SRAM0 off) | on | off | off | off | on |
| P1.7 | Low Power (XIP & SRAM0 & SRAM1 off) | on | off | off | off | off |
| OFF | Not Powered | off | off | off | off | off |

*Table 475. supported*

6.2. Power management
444

RP2350 Datasheet

In the OFF state, the chip has no external power and all domains are unpowered. The chip moves from OFF to P0.0

automatically as soon as external power is applied.

To determine the current power state, read the STATE.CURRENT field. CURRENT is a 4 bit field representing the power

state of the switched core and memory power domains.
