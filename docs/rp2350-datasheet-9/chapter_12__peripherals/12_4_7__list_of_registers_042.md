---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.4.7. List of registers
pages: 1074-1077
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 12.4.7. List of registers

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

*Table 1120. List of*

12.4. ADC and Temperature Sensor
1073

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

*Table 1121. CS ADC: RESULT Register Offset: 0x04*

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:12 | Reserved. | - | - |
| 11:0 | Result of most recent ADC conversion | RO | 0x000 |
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

*Table 1122. RESULT ADC: FCS Register Offset: 0x08 Description FIFO control and status*

12.4. ADC and Temperature Sensor
1074

RP2350 Datasheet

*Table 1123. FCS ADC: FIFO Register Offset: 0x0c Description Conversion result FIFO*

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:16 | Reserved. | - | - |
| 15 | ERR: 1 if this particular sample experienced a conversion error. Remains in the same location if the sample is shifted. | RF | - |
| 14:12 | Reserved. | - | - |
| 11:0 | VAL | RF | - |

*Table 1124. FIFO ADC: DIV Register Offset: 0x10 Description*

Clock divider. If non-zero, CS_START_MANY will start conversions

at regular intervals rather than back-to-back.

The divider is reset when either of these fields are written.

Total period is 1 + INT + FRAC / 256

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:24 | Reserved. | - | - |
| 23:8 | INT: Integer part of clock divisor. | RW | 0x0000 |
| 7:0 | FRAC: Fractional part of clock divisor. First-order delta-sigma. | RW | 0x00 |
| 31:1 | Reserved. | - | - |
| 0 | FIFO: Triggered when the sample FIFO reaches a certain level. This level can be programmed via the FCS_THRESH field. | RO | 0x0 |

*Table 1125. DIV*

12.4. ADC and Temperature Sensor
1075

RP2350 Datasheet

ADC: INTR Register

Offset: 0x14

Description

Raw Interrupts

*Table 1126. INTR ADC: INTE Register Offset: 0x18 Description Interrupt Enable*

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:1 | Reserved. | - | - |
| 0 | FIFO: Triggered when the sample FIFO reaches a certain level. This level can be programmed via the FCS_THRESH field. | RW | 0x0 |

*Table 1127. INTE ADC: INTF Register Offset: 0x1c Description Interrupt Force*

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:1 | Reserved. | - | - |
| 0 | FIFO: Triggered when the sample FIFO reaches a certain level. This level can be programmed via the FCS_THRESH field. | RW | 0x0 |

*Table 1128. INTF ADC: INTS Register Offset: 0x20 Description Interrupt status after masking & forcing*

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:1 | Reserved. | - | - |
| 0 | FIFO: Triggered when the sample FIFO reaches a certain level. This level can be programmed via the FCS_THRESH field. | RO | 0x0 |

*Table 1129. INTS*

## Embedded Images

![img_p1074_00.png](images/img_p1074_00.png)

