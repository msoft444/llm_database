---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.4.1. Frequency accuracy and calibration
pages: 570-570
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 8.4.1. Frequency accuracy and calibration

ROSC: RANDOMBIT Register
Offset: 0x20
Table 614.
RANDOMBIT Register
Bits
Description
Type
Reset
31:1
Reserved.
-
-
0
This just reads the state of the oscillator output so randomness is
compromised if the ring oscillator is stopped or run at a harmonic of the bus
frequency
RO
0x1
ROSC: COUNT Register
Offset: 0x24
Table 615. COUNT
Register
Bits
Description
Type
Reset
31:16
Reserved.
-
-
15:0
A down counter running at the ROSC frequency which counts to zero and
stops.
To start the counter write a non-zero value.
Can be used for short software pauses when setting up time sensitive
hardware.
RW
0x0000
8.4. Low Power oscillator (LPOSC)
The Low Power Oscillator (LPOSC) provides a clock signal to the always-on logic when the main crystal oscillator is
powered down in a low power (P1.x) state. It operates at a nominal 32.768kHz and is an RC oscillator, requiring no
external components. The oscillator’s output clock is used to sequence initial chip start up and transition to and from
low-power states. It can also be used by the AON Timer, see Section 12.10, “Always-on timer”.
The oscillator starts up as soon as the core power supply is available and power-on reset has been released. If
brownout detection is enabled, the oscillator will be disabled when a core supply brownout is detected, but will restart
as soon as the core supply has recovered and brownout reset has been released. The oscillator’s frequency takes
around 1ms to stabilise, and the chip will be held in reset during this period.
8.4.1. Frequency accuracy and calibration
The low power oscillator has an initial frequency accuracy of ±20%. However, it can be trimmed to ±1.5% using the TRIM
field in the LPOSC register. 63 trim steps are available, each between 1% and 3% of the oscillator’s initial frequency. The
frequency can be trimmed down by 32 steps or up by 31 steps. See Table 616, “low power oscillator output frequency
and trimming” and Section 8.4.3, “List of registers” for details.
Table 616. low power
oscillator output
frequency and
trimming
Parameter
Description
Min
Typ
Max
Units
F0.initial
initial output
frequency
26.2144
32.768
39.3216
kHz
trimSTEP
frequency trim
step
-
1
3
% of initial output
frequency
F0.trimmed
trimmed output
frequency
32.27648
32.768
33.25952
kHz
Frequency drift with temperature: ±14%.
RP2350 Datasheet
8.4. Low Power oscillator (LPOSC)
569

