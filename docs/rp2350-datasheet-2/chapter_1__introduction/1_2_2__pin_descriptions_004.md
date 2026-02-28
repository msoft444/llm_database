---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 1. Introduction
section: 1.2.2. Pin descriptions
pages: 17-17
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 1.2.2. Pin descriptions

![Page 17 figure](images/fig_p0017.png)

RP2350 Datasheet

1.2.1.2. QFN-80 (RP2350B)

Figure 3. RP2350

Pinout for QFN-80

10×10mm (reduced

|  |  | GPIO3 | GPIO2 | GPIO1 | GPIO0 | IOVDD | QSPI_SS | QSPI_SD1 | QSPI_SD2 | QSPI_SD0 | QSPI_SCLK | QSPI_SD3 | QSPI_IOVDD | USB_OTP_VDD | USB_DP | USB_DM | VREG_FB | VREG_VIN | VREG_LX | VREG_PGND | VREG_AVDD |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  | 80 | 79 | 78 | 77 | 76 | 75 | 74 | 73 | 72 | 71 | 70 | 69 | 68 | 67 | 66 | 65 | 64 | 63 | 62 | 61 |  |  |
| GPIO4 | 1 | GND
TOP VIEW |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 60 | IOVDD |
| GPIO5 | 2 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 59 | ADC_AVDD |
| GPIO6 | 3 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 58 | GPIO47_ADC7 |
| GPIO7 | 4 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 57 | GPIO46_ADC6 |
| IOVDD | 5 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 56 | GPIO45_ADC5 |
| GPIO8 | 6 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 55 | GPIO44_ADC4 |
| GPIO9 | 7 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 54 | GPIO43_ADC3 |
| GPIO10 | 8 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 53 | GPIO42_ADC2 |
| GPIO11 | 9 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 52 | GPIO41_ADC1 |
| DVDD | 10 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 51 | DVDD |
| GPIO12 | 11 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 50 | IOVDD |
| GPIO13 | 12 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 49 | GPIO40_ADC0 |
| GPIO14 | 13 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 48 | GPIO39 |
| GPIO15 | 14 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 47 | GPIO38 |
| GIPOIVOD1D1 | 15 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 46 | GPIO37 |
| GPIO16 | 16 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 45 | GPIO36 |
| GPIO17 | 17 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 44 | GPIO35 |
| GPIO18 | 18 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 43 | GPIO34 |
| GPIO19 | 19 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 42 | GPIO33 |
| GPIO20 | 20 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 41 | IOVDD |
|  |  | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 |  |  |
|  |  | GPIO21 | GPIO22 | GPIO23 | IOVDD | GPIO24 | GPIO25 | GPIO26 | GPIO27 | IOVDD | XIN | XOUT | DVDD | SWCLK | SWDIO | RUN | GPIO28 | GPIO29 | GPIO30 | GPIO31 | GPIO32 |  |  |

ePad size)

1.2.2. Pin descriptions

| Name | Description |
| --- | --- |
| GPIOx | General-purpose digital input and output. RP2350 can connect one of a number of internal
peripherals to each GPIO, or control GPIOs directly from software. |
| GPIOx/ADCy | General-purpose digital input and output, with analogue-to-digital converter function. The RP2350
ADC has an analogue multiplexer which can select any one of these pins, and sample the voltage. |
| QSPIx | Interface to a SPI, Dual-SPI or Quad-SPI flash or PSRAM device, with execute-in-place support.
These pins can also be used as software-controlled GPIOs, if they are not required for flash
access. |
| USB_DM and
USB_DP | USB controller, supporting Full Speed device and Full/Low Speed host. A 27Ω series termination
resistor is required on each pin, but bus pullups and pulldowns are provided internally. These pins
can be used as software-controlled GPIOs, if USB is not required. |
| XIN and XOUT | Connect a crystal to RP2350’s crystal oscillator. XIN can also be used as a single-ended CMOS
clock input, with XOUT disconnected. The USB bootloader defaults to a 12MHz crystal or 12MHz
clock input, but this can be configured via OTP. |
| RUN | Global asynchronous reset pin. Reset when driven low, run when driven high. If no external reset is
required, this pin can be tied directly to IOVDD. |
| SWCLK and
SWDIO | Access to the internal Serial Wire Debug multi-drop bus. Provides debug access to both
processors, and can be used to download code. |
| GND | Single external ground connection, bonded to a number of internal ground pads on the RP2350 die. |

Table 2. The function

of each pin is briefly

described here. Full

electrical

specifications can be

found in Chapter 14.

1.2. Pinout reference
16
