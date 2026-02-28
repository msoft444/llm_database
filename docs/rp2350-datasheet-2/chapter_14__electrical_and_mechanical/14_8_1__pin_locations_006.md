---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 14. Electrical and mechanical
section: 14.8.1. Pin locations
pages: 1334-1335
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 14.8.1. Pin locations

RP2350 Datasheet

14.7. Compliance

RP2350 QFN-60 is compliant to Moisture Sensitivity Level 1. The Moisture Sensitivity Level compliance of RP2350 QFN-

80 is yet to be fully characterised, and details will follow in a future datasheet update.

RP2350 is compliant to the requirement of REACH Substances of Very High Concern (SVHC), EU ECHA directive.

RP2350 is compliant to the requirement and standard of Controlled Environment-related Substance of RoHS directive

(EU) 2011/65/EU and directive (EU) 2015/863.

Raspberry Pi Ltd carried out the following Package Level reliability qualifications on RP2350:

• Temperature Cycling per JESD22-A104
• HAST per JESD22-A110
• HTSL per JESD22-A103
• MSL level per JESD22-A113

The following Silicon Level reliability qualification were also carried out:

• HTOL per JESD22-A108F

NOTE

A tin whiskers test is not performed. RP2350 is a bottom-only termination device in the QFN-60 and QFN-80

packages, therefore JEDEC standard (JESD201A) is not applicable.

14.8. Pinout

14.8.1. Pin locations

14.8.1.1. QFN-60 (RP2350A)

14.7. Compliance
1333

![Page 1335 figure](images/fig_p1335.png)

RP2350 Datasheet

Figure 148. RP2350

Pinout for QFN-60

7×7mm

|  |  | QSPI_SS | QSPI_SD1 | QSPI_SD2 | QSPI_SD0 | QSPI_SCLK | QSPI_SD3 | QSPI_IOVDD | USB_OTP_VDD | USB_DP | USB_DM | VREG_FB | VREG_VIN | VREG_LX | VREG_PGND | VREG_AVDD |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  | 60 | 59 | 58 | 57 | 56 | 55 | 54 | 53 | 52 | 51 | 50 | 49 | 48 | 47 | 46 |  |  |
| IOVDD | 1 | GND
TOP VIEW |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 45 | IOVDD |
| GPIO0 | 2 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 44 | ADC_AVDD |
| GPIO1 | 3 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 43 | GPIO29_ADC3 |
| GPIO2 | 4 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 42 | GPIO28_ADC2 |
| GPIO3 | 5 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 41 | GPIO27_ADC1 |
| DVDD | 6 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 40 | GPIO26_ADC0 |
| GPIO4 | 7 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 39 | DVDD |
| GPIO5 | 8 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 38 | IOVDD |
| GPIO6 | 9 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 37 | GPIO25 |
| GPIO7 | 10 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 36 | GPIO24 |
| IOVDD | 11 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 35 | GPIO23 |
| GPIO8 | 12 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 34 | GPIO22 |
| GPIO9 | 13 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 33 | GPIO21 |
| GPIO10 | 14 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 32 | GPIO20 |
| GPIO11 | 15 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 31 | GPIO19 |
|  |  | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 |  |  |
|  |  | GPIO12 | GPIO13 | GPIO14 | GPIO15 | IOVDD | XIN | XOUT | DVDD | SWCLK | SWDIO | RUN | GPIO16 | GPIO17 | GPIO18 | IOVDD |  |  |

14.8.1.2. QFN-80 (RP2350B)

Figure 149. RP2350

Pinout for QFN-80

10×10mm

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

14.8. Pinout
1334
