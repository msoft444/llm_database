---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 1. Introduction
section: 1.3. Why is the chip called RP2350?
pages: 23-23
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 1.3. Why is the chip called RP2350?

![Page 23 figure](images/fig_p0023.png)

RP2350 Datasheet

Pin
F0
F1
F2
F3
F4
F5
F6
F7
F8
F9
F10
F11

USB DM
UART1 RX
I2C0 SCL
SIO

QSPI SCK
QMI SCK
UART1 CTS
I2C1 SDA
SIO
UART1 TX

QSPI CSn
QMI CS0n
UART1 RTS
I2C1 SCL
SIO
UART1 RX

QSPI SD0
QMI SD0
UART0 TX
I2C0 SDA
SIO

QSPI SD1
QMI SD1
UART0 RX
I2C0 SCL
SIO

QSPI SD2
QMI SD2
UART0 CTS
I2C1 SDA
SIO
UART0 TX

QSPI SD3
QMI SD3
UART0 RTS
I2C1 SCL
SIO
UART0 RX

| Function Name | Description |
| --- | --- |
| UARTx | Connect one of the internal PL011 UART peripherals to GPIO |
| I2Cx | Connect one of the internal DW I2C peripherals to GPIO |
| SIO | Software control of GPIO, from the single-cycle IO (SIO) block. The SIO function (F5) must be selected for the processors to drive a GPIO, but the input is always connected, so software can check the state of GPIOs at any time. |
| QMI | QSPI memory interface peripheral, used for execute-in-place from external QSPI flash or PSRAM memory devices. |

Table 6. GPIO bank 1

1.3. Why is the chip called RP2350?

Figure 4. An

explanation for the

RP 2  3  5  0 

name of the RP2350

chip.

floor(log2(nonvolatile / 128 kB))

floor(log2(RAM / 16 kB))

Type of core (e.g. Cortex-M33)

Number of cores

Raspberry Pi

The post-fix numeral on RP2350 comes from the following,

1. Number of processor cores

◦2 indicates a dual-core system

2. Loosely which type of processor

◦3 indicates Cortex-M33 or Hazard3

3. Internal memory capacity: 

◦5 indicates at least 25 × 16 kB = 512 kB

◦RP2350 has 520 kB of main system SRAM

4. Internal storage capacity: 
 (or 0 if no onboard nonvolatile storage)

1.3. Why is the chip called RP2350?
22

## Embedded Images

![img_p0023_00.png](images/img_p0023_00.png)

![img_p0023_01.png](images/img_p0023_01.png)

