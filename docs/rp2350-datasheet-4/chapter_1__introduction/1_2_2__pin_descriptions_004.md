---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 1. Introduction
section: 1.2.2. Pin descriptions
pages: 17-17
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 1.2.2. Pin descriptions

![Page 17 figure](images/fig_p0017.png)

1.2.2. Pin descriptions

| Name | Description |
| --- | --- |
| GPIOx | General-purpose digital input and output. RP2350 can connect one of a number of internal peripherals to each GPIO, or control GPIOs directly from software. |
| GPIOx/ADCy | General-purpose digital input and output, with analogue-to-digital converter function. The RP2350 ADC has an analogue multiplexer which can select any one of these pins, and sample the voltage. |
| QSPIx | Interface to a SPI, Dual-SPI or Quad-SPI flash or PSRAM device, with execute-in-place support. These pins can also be used as software-controlled GPIOs, if they are not required for flash access. |
| USB_DM and USB_DP | USB controller, supporting Full Speed device and Full/Low Speed host. A 27Ω series termination resistor is required on each pin, but bus pullups and pulldowns are provided internally. These pins can be used as software-controlled GPIOs, if USB is not required. |
| XIN and XOUT | Connect a crystal to RP2350’s crystal oscillator. XIN can also be used as a single-ended CMOS clock input, with XOUT disconnected. The USB bootloader defaults to a 12MHz crystal or 12MHz clock input, but this can be configured via OTP. |
| RUN | Global asynchronous reset pin. Reset when driven low, run when driven high. If no external reset is required, this pin can be tied directly to IOVDD. |
| SWCLK and SWDIO | Access to the internal Serial Wire Debug multi-drop bus. Provides debug access to both processors, and can be used to download code. |
| GND | Single external ground connection, bonded to a number of internal ground pads on the RP2350 die. |

Table 2. The function

of each pin is briefly

described here. Full

electrical

specifications can be

found in Chapter 14.

1.2. Pinout reference
16
