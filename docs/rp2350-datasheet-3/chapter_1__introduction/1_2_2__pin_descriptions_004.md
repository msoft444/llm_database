---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 1. Introduction
section: 1.2.2. Pin descriptions
pages: 17-17
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 1.2.2. Pin descriptions

![Page 17 figure](images/fig_p0017.png)

RP2350 Datasheet

1.2.1.2. QFN-80 (RP2350B)

Figure 3. RP2350

Pinout for QFN-80

10×10mm (reduced

USB_OTP_VDD

VREG_PGND

ePad size)

QSPI_IOVDD

VREG_AVDD

QSPI_SCLK

VREG_VIN

QSPI_SD0

QSPI_SD2

QSPI_SD1

QSPI_SD3

VREG_FB

VREG_LX

QSPI_SS

USB_DM

USB_DP

IOVDD

GPIO0

GPIO1

GPIO2

GPIO3

80 79 78 77 76 75 74 73 72 71 70 69 68

67 66 65 64 63 62 61

1

60

GPIO4

IOVDD

2

59

GPIO5

ADC_AVDD

3

58

GPIO6

GPIO47_ADC7

4

57

GPIO7

GPIO46_ADC6

5

56

IOVDD

GPIO45_ADC5

6

55

GPIO8

GPIO44_ADC4

7

54

GPIO9

GPIO43_ADC3

8

53

GPIO10

GPIO42_ADC2

9

52

GPIO11

GPIO41_ADC1

GND

10

51

DVDD

DVDD

11

50

GPIO12

IOVDD

12

49

GPIO13

GPIO40_ADC0

13

48

GPIO14

GPIO39

GPIO15

14

GPIO38

47

15

15

IOVDD

GPIO11

GPIO37
46

TOP VIEW

16

GPIO16

45

GPIO36

17

GPIO17

44

GPIO35

GPIO18

18

43

GPIO34

19

GPIO19

42

GPIO33

GPIO20 20

IOVDD
41

21 22 23 24 25 26 27 28 29 30 31 32 33 34 35

36 37 38 39 40

RUN

XIN

XOUT

GPIO32

GPIO31

GPIO30

GPIO29

GPIO28

GPIO27

GPIO26

GPIO25

GPIO24

GPIO23

GPIO22

GPIO21

SWCLK

DVDD

IOVDD

IOVDD

SWDIO

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
