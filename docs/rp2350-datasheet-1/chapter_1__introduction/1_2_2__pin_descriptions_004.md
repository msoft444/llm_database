---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 1. Introduction
section: 1.2.2. Pin descriptions
pages: 17-17
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 1.2.2. Pin descriptions

1.2.1.2. QFN-80 (RP2350B)
VREG_AVDD
VREG_PGND
VREG_LX
VREG_VIN
VREG_FB
USB_DM
USB_DP
RUN
SWDIO
GPIO11
GPIO15
GND
TOP VIEW
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
GPIO14
GPIO13
GPIO12
GPIO19
GPIO18
15
16
17
18
19
GPIO20 20
GPIO17
GPIO16
IOVDD
DVDD
GPIO11
GPIO10
GPIO9
GPIO8
IOVDD
GPIO7
GPIO6
GPIO5
GPIO4
GPIO38
GPIO39
GPIO40_ADC0
IOVDD
DVDD
GPIO41_ADC1
GPIO42_ADC2
GPIO43_ADC3
GPIO44_ADC4
GPIO45_ADC5
GPIO46_ADC6
GPIO47_ADC7
ADC_AVDD
IOVDD
USB_OTP_VDD
QSPI_IOVDD
QSPI_SD3
QSPI_SCLK
QSPI_SD0
QSPI_SD2
QSPI_SD1
QSPI_SS
IOVDD
GPIO0
GPIO1
GPIO2
GPIO3
60
80 79 78 77 76 75 74 73 72 71 70 69 68
21 22 23 24 25 26 27 28 29 30 31 32 33 34 35
59
58
57
56
55
54
53
52
51
50
49
48
47
SWCLK
DVDD
XOUT
GPIO32
GPIO31
36 37 38 39 40
GPIO30
GPIO29
GPIO28
XIN
IOVDD
GPIO27
GPIO26
GPIO25
GPIO24
IOVDD
GPIO23
GPIO22
GPIO21
67 66 65 64 63 62 61
GPIO33
GPIO34
GPIO35
GPIO36
GPIO37
46
45
44
43
42
IOVDD
41
Figure 3. RP2350
Pinout for QFN-80
10×10mm (reduced
ePad size)
1.2.2. Pin descriptions
Table 2. The function
of each pin is briefly
described here. Full
electrical
specifications can be
found in Chapter 14.
Name
Description
GPIOx
General-purpose digital input and output. RP2350 can connect one of a number of internal
peripherals to each GPIO, or control GPIOs directly from software.
GPIOx/ADCy
General-purpose digital input and output, with analogue-to-digital converter function. The RP2350
ADC has an analogue multiplexer which can select any one of these pins, and sample the voltage.
QSPIx
Interface to a SPI, Dual-SPI or Quad-SPI flash or PSRAM device, with execute-in-place support.
These pins can also be used as software-controlled GPIOs, if they are not required for flash
access.
USB_DM and
USB_DP
USB controller, supporting Full Speed device and Full/Low Speed host. A 27Ω series termination
resistor is required on each pin, but bus pullups and pulldowns are provided internally. These pins
can be used as software-controlled GPIOs, if USB is not required.
XIN and XOUT
Connect a crystal to RP2350’s crystal oscillator. XIN can also be used as a single-ended CMOS
clock input, with XOUT disconnected. The USB bootloader defaults to a 12MHz crystal or 12MHz
clock input, but this can be configured via OTP.
RUN
Global asynchronous reset pin. Reset when driven low, run when driven high. If no external reset is
required, this pin can be tied directly to IOVDD.
SWCLK and
SWDIO
Access to the internal Serial Wire Debug multi-drop bus. Provides debug access to both
processors, and can be used to download code.
GND
Single external ground connection, bonded to a number of internal ground pads on the RP2350 die.
RP2350 Datasheet
1.2. Pinout reference
16

