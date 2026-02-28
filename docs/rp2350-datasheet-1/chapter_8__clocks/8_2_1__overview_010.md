---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.2.1. Overview
pages: 555-556
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 8.2.1. Overview

Description
Raw Interrupts
Table 593. INTR
Register
Bits
Description
Type
Reset
31:1
Reserved.
-
-
0
CLK_SYS_RESUS
RO
0x0
CLOCKS: INTE Register
Offset: 0xc8
Description
Interrupt Enable
Table 594. INTE
Register
Bits
Description
Type
Reset
31:1
Reserved.
-
-
0
CLK_SYS_RESUS
RW
0x0
CLOCKS: INTF Register
Offset: 0xcc
Description
Interrupt Force
Table 595. INTF
Register
Bits
Description
Type
Reset
31:1
Reserved.
-
-
0
CLK_SYS_RESUS
RW
0x0
CLOCKS: INTS Register
Offset: 0xd0
Description
Interrupt status after masking & forcing
Table 596. INTS
Register
Bits
Description
Type
Reset
31:1
Reserved.
-
-
0
CLK_SYS_RESUS
RO
0x0
8.2. Crystal oscillator (XOSC)
8.2.1. Overview
RP2350 Datasheet
8.2. Crystal oscillator (XOSC)
554


Figure 38. The XOSC
is an amplifier. When
a piezoelectric crystal
is connected across
XIN and XOUT, the
amplified feedback
drives the crystal into
mechanical
resonance. This
creates a precise
reference for on-chip
clock generation.
External signals can
also be driven directly
into XIN.
The Crystal Oscillator (XOSC) uses an external crystal to produce an accurate reference clock. RP2350 supports 1 MHz
to 50 MHz crystals and the RP2350 reference design (see Hardware design with RP2350, Minimal Design Example)
uses a 12 MHz crystal. The reference clock is distributed to the PLLs, which can be used to multiply the XOSC frequency
to provide accurate high speed clocks. For example, they can generate a 48 MHz clock which meets the frequency
accuracy requirement of the USB interface and a 150 MHz maximum speed system clock. The XOSC clock is also a
clock source for the clock generators and can be used directly if required.
If the user already has an accurate clock source, it is possible to drive an external clock directly into XIN (aka XI), and
disable the oscillator circuit. In this mode XIN can be driven at up to 50 MHz.
To use XOSC clock externally, output it to a GPIO pin using one of the clk_gpclk0-clk_gpclk3 generators. You cannot take
XOSC output directly from the XIN (XI) or XOUT (XO) pins.
NOTE
A minimum crystal frequency of 5 MHz is needed for the PLL. See Section 8.6, “PLL”.
8.2.1.1. Recommended crystals
For the best performance and stability across typical operating temperature ranges, it is recommended to use the
Abracon ABM8-272-T3. You can source the ABM8-272-T3 directly from Abracon or from an authorised reseller. The
Abracon ABM8-272-T3 has the following specifications:
Table 597. Key Crystal
Specifications.
Parameters
Minimum
Typical
Maximum
Units
Notes
Center Frequency
12.000
12.000
12.000
MHz
Operation Mode
Fundamental-AT
Fundamental-AT
Fundamental-AT
Operating Temperature
-40
+85
°C
Storage Temperature
-55
+125
°C
Frequency Tolerance (25 °C)
-30
+30
ppm
Frequency Stability (25 °C)
-30
+30
ppm
Equivalent Series Resistance (R1)
50
Ω
Shunt Capacitance (C0)
3.0
pF
Load Capacitance (CL)
10
10
10
pF
Drive Level
10
200
μW
Aging
-5
+5
ppm
@25±3 °C, 1st year
Insulation Resistance
500
MΩ
@100 Vdc±15 V
Even if you use a crystal with similar specifications, you will need to test the circuit over a range of temperatures to
RP2350 Datasheet
8.2. Crystal oscillator (XOSC)
555

