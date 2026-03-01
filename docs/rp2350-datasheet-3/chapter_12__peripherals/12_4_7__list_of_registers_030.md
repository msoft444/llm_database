---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.4.7. List of registers
pages: 1074-1076
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.4.7. List of registers

![Page 1074 figure](images/fig_p1074.png)

RP2350 Datasheet

12.4.4. ADC ENOB

ADC ENOB details are shown in Table 1438.

12.4.5. INL and DNL

Details to follow.

12.4.6. Temperature sensor

The temperature sensor measures the Vbe voltage of a biased bipolar diode, connected to the fifth ADC channel (

AINSEL=4) on QFN-60 or the ninth ADC channel (AINSEL=8) on QFN-80. Typically, Vbe = 0.706 V at 27 °C, with a slope of

-1.721 mV per degree. Therefore the temperature in °C can be approximated as follows:

As the Vbe and the Vbe slope can vary over the temperature range, and from device to device, some user calibration may

be required if accurate measurements are required.

The temperature sensor’s bias source must be enabled before use, via CS.TS_EN. This increases current consumption

on ADC_AVDD by approximately 40 μA.

NOTE

The on board temperature sensor is very sensitive to errors in reference voltage. At 3.3 V, a value of 891 returned by

the ADC corresponds to a temperature of 20.1°C. At a reference voltage 1% lower than 3.3 V, the same reading of

891 correspond to a temperature of 24.3°C: a temperature change of over 4°C. To improve the accuracy of the

internal temperature sensor, consider adding an external reference voltage.

12.4.7. List of registers

The ADC registers start at a base address of 0x400a0000 (defined as ADC_BASE in SDK).

| Offset | Name | Info |
| --- | --- | --- |
| 0x00 | CS | ADC Control and Status |
| 0x04 | RESULT | Result of most recent ADC conversion |
| 0x08 | FCS | FIFO control and status |
| 0x0c | FIFO | Conversion result FIFO |
| 0x10 | DIV | Clock divider. If non-zero, CS_START_MANY will start conversions at regular intervals rather than back-to-back. The divider is reset when either of these fields are written. Total period is 1 + INT + FRAC / 256 |
| 0x14 | INTR | Raw Interrupts |
| 0x18 | INTE | Interrupt Enable |
| 0x1c | INTF | Interrupt Force |
| 0x20 | INTS | Interrupt status after masking & forcing |

Table 1120. List of

12.4. ADC and Temperature Sensor
1073

![Page 1075 figure](images/fig_p1075.png)

RP2350 Datasheet

ADC: CS Register

Offset: 0x00

Description

ADC Control and Status

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:25 | Reserved. | - | - |
| 24:16 | RROBIN: Round-robin sampling. 1 bit per channel. Set all bits to 0 to disable. Otherwise, the ADC will cycle through each enabled channel in a round-robin fashion. The first channel to be sampled will be the one currently indicated by AINSEL. AINSEL will be updated after each conversion with the newly-selected channel. | RW | 0x000 |
| 15:12 | AINSEL: Select analog mux input. Updated automatically in round-robin mode. This is corrected for the package option so only ADC channels which are bonded are available, and in the correct order | RW | 0x0 |
| 11 | Reserved. | - | - |
| 10 | ERR_STICKY: Some past ADC conversion encountered an error. Write 1 to clear. | WC | 0x0 |
| 9 | ERR: The most recent ADC conversion encountered an error; result is undefined or noisy. | RO | 0x0 |
| 8 | READY: 1 if the ADC is ready to start a new conversion. Implies any previous conversion has completed. 0 whilst conversion in progress. | RO | 0x0 |
| 7:4 | Reserved. | - | - |
| 3 | START_MANY: Continuously perform conversions whilst this bit is 1. A new conversion will start immediately after the previous finishes. | RW | 0x0 |
| 2 | START_ONCE: Start a single conversion. Self-clearing. Ignored if start_many is asserted. | SC | 0x0 |
| 1 | TS_EN: Power on temperature sensor. 1 - enabled. 0 - disabled. | RW | 0x0 |
| 0 | EN: Power on ADC and enable its clock. 1 - enabled. 0 - disabled. | RW | 0x0 |

Table 1121. CS

ADC: RESULT Register

Offset: 0x04

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:12 | Reserved. | - | - |
| 11:0 | Result of most recent ADC conversion | RO | 0x000 |

Table 1122. RESULT

ADC: FCS Register

Offset: 0x08

Description

FIFO control and status

12.4. ADC and Temperature Sensor
1074

![Page 1076 figure](images/fig_p1076.png)

RP2350 Datasheet

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:28 | Reserved. | - | - |
| 27:24 | THRESH: DREQ/IRQ asserted when level >= threshold | RW | 0x0 |
| 23:20 | Reserved. | - | - |
| 19:16 | LEVEL: The number of conversion results currently waiting in the FIFO | RO | 0x0 |
| 15:12 | Reserved. | - | - |
| 11 | OVER: 1 if the FIFO has been overflowed. Write 1 to clear. | WC | 0x0 |
| 10 | UNDER: 1 if the FIFO has been underflowed. Write 1 to clear. | WC | 0x0 |
| 9 | FULL | RO | 0x0 |
| 8 | EMPTY | RO | 0x0 |
| 7:4 | Reserved. | - | - |
| 3 | DREQ_EN: If 1: assert DMA requests when FIFO contains data | RW | 0x0 |
| 2 | ERR: If 1: conversion error bit appears in the FIFO alongside the result | RW | 0x0 |
| 1 | SHIFT: If 1: FIFO results are right-shifted to be one byte in size. Enables DMA to byte buffers. | RW | 0x0 |
| 0 | EN: If 1: write result to the FIFO after each conversion. | RW | 0x0 |

Table 1123. FCS

ADC: FIFO Register

Offset: 0x0c

Description

Conversion result FIFO

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:16 | Reserved. | - | - |
| 15 | ERR: 1 if this particular sample experienced a conversion error. Remains in the same location if the sample is shifted. | RF | - |
| 14:12 | Reserved. | - | - |
| 11:0 | VAL | RF | - |

Table 1124. FIFO

ADC: DIV Register

Offset: 0x10

Description

Clock divider. If non-zero, CS_START_MANY will start conversions

at regular intervals rather than back-to-back.

The divider is reset when either of these fields are written.

Total period is 1 + INT + FRAC / 256

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:24 | Reserved. | - | - |
| 23:8 | INT: Integer part of clock divisor. | RW | 0x0000 |
| 7:0 | FRAC: Fractional part of clock divisor. First-order delta-sigma. | RW | 0x00 |

Table 1125. DIV

12.4. ADC and Temperature Sensor
1075

## Embedded Images

![img_p1074_00.png](images/img_p1074_00.png)

