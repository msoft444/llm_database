---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.4.1. Changes from RP2040
pages: 1069-1069
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 12.4.1. Changes from RP2040

ADC
0
1
2
3
4
5
6
7
8
Analogue input
ain_sel
Digital pad
GPIO[40]
Analogue input
Digital pad
GPIO[41]
Analogue input
Digital pad
GPIO[42]
Analogue input
Digital pad
GPIO[43]
Analogue input
Digital pad
GPIO[44]
Analogue input
Digital pad
GPIO[45]
Analogue input
Digital pad
GPIO[46]
Temperature 
Sensor 
(on chip)
Analogue input
Digital pad
GPIO[47]
Figure 108. ADC
Connection Diagram
for QFN-80. This
package features
eight external ADC
inputs (0 through 7),
on Bank 0 GPIOs 40
through 47. The
internal temperature
sensor connects to a
ninth channel (channel
8). Like in QFN-60,
each ADC input shares
a package pin with a
digital Bank 0 GPIO:
generally the digital
functions are disabled
when the ADC is in
use.
When using an ADC input shared with a GPIO pin, always disable the pin’s digital functions by setting IE low and OD high
in the pin’s pad control register. See Section 9.11.3, “Pad Control - User Bank” for details.
The maximum ADC input voltage is determined by the digital IO supply voltage (IOVDD), not the ADC supply voltage
(ADC_AVDD). For example, if IOVDD is powered at 1.8 V, the voltage on the ADC inputs should not exceed 1.8 V + 10% even if
ADC_AVDD is powered at 3.3 V. Voltages greater than IOVDD will result in leakage currents through the ESD protection
diodes. See Section 14.9, “Electrical specifications” for details.
12.4.1. Changes from RP2040
• Removed spikes in differential nonlinearity at codes 0x200, 0x600, 0xa00 and 0xe00, as documented by erratum
RP2040-E11, improving the ADC’s precision by around 0.5 ENOB.
RP2350 Datasheet
12.4. ADC and Temperature Sensor
1068

