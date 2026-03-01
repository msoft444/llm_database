---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.4.1. Changes from RP2040
pages: 1069-1069
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.4.1. Changes from RP2040

![Page 1069 figure](images/fig_p1069.png)

RP2350 Datasheet

Figure 108. ADC

ain_sel

Connection Diagram

for QFN-80. This

package features

eight external ADC

Analogue input

0

inputs (0 through 7),

on Bank 0 GPIOs 40

Digital pad
GPIO[40]

through 47. The

internal temperature

sensor connects to a

Analogue input

ninth channel (channel

1

8). Like in QFN-60,

each ADC input shares

Digital pad
GPIO[41]

a package pin with a

digital Bank 0 GPIO:

generally the digital

Analogue input

2

functions are disabled

when the ADC is in

use.

Digital pad
GPIO[42]

Analogue input

3

Digital pad
GPIO[43]

Analogue input

ADC

4

Digital pad
GPIO[44]

Analogue input

5

Digital pad
GPIO[45]

Analogue input

6

Digital pad
GPIO[46]

Analogue input

7

Digital pad
GPIO[47]

Temperature 

8

Sensor 
(on chip)

When using an ADC input shared with a GPIO pin, always disable the pin’s digital functions by setting IE low and OD high

in the pin’s pad control register. See Section 9.11.3, “Pad Control - User Bank” for details.

The maximum ADC input voltage is determined by the digital IO supply voltage (IOVDD), not the ADC supply voltage

(ADC_AVDD). For example, if IOVDD is powered at 1.8 V, the voltage on the ADC inputs should not exceed 1.8 V + 10% even if

ADC_AVDD is powered at 3.3 V. Voltages greater than IOVDD will result in leakage currents through the ESD protection

diodes. See Section 14.9, “Electrical specifications” for details.

12.4.1. Changes from RP2040

• Removed spikes in differential nonlinearity at codes 0x200, 0x600, 0xa00 and 0xe00, as documented by erratum

RP2040-E11, improving the ADC’s precision by around 0.5 ENOB.

12.4. ADC and Temperature Sensor
1068
