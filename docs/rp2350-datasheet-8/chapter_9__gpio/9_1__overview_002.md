---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.1. Overview
pages: 588-589
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 9.1. Overview

9.1. Overview

RP2350 has up to 54 multi-functional General Purpose Input / Output (GPIO) pins, divided into two banks:

Bank 0

30 user GPIOs in the QFN-60 package (RP2350A), or 48 user GPIOs in the QFN-80 package

Bank 1

six QSPI IOs, and the USB DP/DM pins

You can control each GPIO from software running on the processors, or by a number of other functional blocks. To

meet USB rise and fall specifications, the analogue characteristics of the USB pins differ from the GPIO pads. As a

result, we do not include them in the 54 GPIO total. However, you can still use them for UART, I2C, or processor-

controlled GPIO through the single-cycle IO subsystem (SIO).

In a typical use case, the QSPI IOs are used to execute code from an external flash device, leaving 30 or 48 Bank 0

GPIOs for the programmer to use. The QSPI pins might become available for general purpose use when booting the chip

from internal OTP, or controlling the chip externally through SWD in an IO expander application.

All GPIOs support digital input and output. Several Bank 0 GPIOs can also be used as inputs to the chip’s Analogue to

Digital Converter (ADC):

• GPIOs 26 through 29 inclusive (four total) in the QFN-60 package
• GPIOs 40 through 47 (eight total) in the QFN-80 package

Bank 0 supports the following functions:

• Software control via SIO — Section 3.1.3, “GPIO control”
• Programmable IO (PIO) — Chapter 11, PIO
• 2 × SPI — Section 12.3, “SPI”
• 2 × UART — Section 12.1, “UART”
• 2 × I2C (two-wire serial interface) — Section 12.2, “I2C”
• 8 × two-channel PWM in the QFN-60 package, or 12 × in QFN-80  — Section 12.5, “PWM”
• 2 × external clock inputs — Section 8.1.2.4, “External clocks”
• 4 × general purpose clock output — Section 8.1, “Overview”
• 4 × input to ADC in the QFN-60 package, or 8 × in QFN-80 — Section 12.4, “ADC and Temperature Sensor”
• 1 × HSTX high-speed interface — Section 12.11, “HSTX”
• 1 × auxiliary QSPI chip select, for a second XIP device — Section 12.14, “QSPI memory interface (QMI)”
• CoreSight execution trace output — Section 3.5.7, “Trace”
• USB VBUS management — Section 12.7.3.10, “VBUS control”
• External interrupt requests, level or edge-sensitive — Section 9.5, “Interrupts”

Bank 1 contains the QSPI and USB DP/DM pins and supports the following functions:

9.1. Overview
587

RP2350 Datasheet

• Software control via SIO — Section 3.1.3, “GPIO control”
• Flash execute in place (Section 4.4, “External flash and PSRAM (XIP)”) via QSPI Memory Interface (QMI) — Section

12.14, “QSPI memory interface (QMI)”
• UART — Section 12.1, “UART”
• I2C (two-wire serial interface) — Section 12.2, “I2C”

The logical structure of an example IO is shown in Figure 41.

![Page 589 figure](images/fig_p0589.png)

Figure 41. Logical

structure of a GPIO.

Each GPIO can be

controlled by one of a

number of peripherals,

or by software control

registers in the SIO.

The function select

(FSEL) selects which

peripheral output is in

control of the GPIO’s

direction and output

level, and which

peripheral input can

see this GPIO’s input

level. These three

signals (output level,

output enable, input

level) can also be

inverted or forced high

or low, using the GPIO
