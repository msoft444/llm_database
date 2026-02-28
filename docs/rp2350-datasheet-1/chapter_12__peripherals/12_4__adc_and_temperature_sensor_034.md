---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.4. ADC and Temperature Sensor
pages: 1067-1068
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 12.4. ADC and Temperature Sensor

Table 1114.
SSPPCELLID0 Register
Bits
Description
Type
Reset
31:8
Reserved.
-
-
7:0
SSPPCELLID0: These bits read back as 0x0D
RO
0x0d
SPI: SSPPCELLID1 Register
Offset: 0xff4
Description
PrimeCell identification registers, SSPPCellID0-3 on page 3-16
Table 1115.
SSPPCELLID1 Register
Bits
Description
Type
Reset
31:8
Reserved.
-
-
7:0
SSPPCELLID1: These bits read back as 0xF0
RO
0xf0
SPI: SSPPCELLID2 Register
Offset: 0xff8
Description
PrimeCell identification registers, SSPPCellID0-3 on page 3-16
Table 1116.
SSPPCELLID2 Register
Bits
Description
Type
Reset
31:8
Reserved.
-
-
7:0
SSPPCELLID2: These bits read back as 0x05
RO
0x05
SPI: SSPPCELLID3 Register
Offset: 0xffc
Description
PrimeCell identification registers, SSPPCellID0-3 on page 3-16
Table 1117.
SSPPCELLID3 Register
Bits
Description
Type
Reset
31:8
Reserved.
-
-
7:0
SSPPCELLID3: These bits read back as 0xB1
RO
0xb1
12.4. ADC and Temperature Sensor
RP2350 has an internal analogue-digital converter (ADC) with the following features:
• SAR ADC (see Section 12.4.3)
• 500 kS/s (using an independent 48 MHz clock)
• 12-bit with 9.2 ENOB (see Section 12.4.4)
• Five or nine input mux:
◦Four inputs available on QFN-60 package pins shared with GPIO[29:26]
◦Eight inputs available on QFN-80 package pins shared with GPIO[47:40]
◦One input dedicated to the internal temperature sensor (see Section 12.4.6)
RP2350 Datasheet
12.4. ADC and Temperature Sensor
1066


• Eight element receive sample FIFO
• Interrupt generation
• DMA interface (see Section 12.4.3.5)
Figure 107 shows the arrangement of ADC channels in the QFN-60 package. Figure 108 shows the same for QFN-80.
Figure 107. ADC
Connection Diagram
for QFN-60. This
package features four
external ADC inputs (0
through 3), on Bank 0
GPIOs 26 through 29.
The internal
temperature sensor
connects to a fifth
channel (channel 4).
This is functionally the
same ADC
arrangement as
RP2040, although the
underlying hardware is
different, to support
the additional
channels on QFN-80.
RP2350 Datasheet
12.4. ADC and Temperature Sensor
1067

