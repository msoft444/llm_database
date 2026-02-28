---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 1. Introduction
section: 1.2.1. Pin locations
pages: 16-16
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 1.2.1. Pin locations

RP2350 Datasheet

switched mode buck converter when the system is awake, providing up to 200 mA at a variable output voltage, and can

switch to a low-quiescent-current LDO mode when the system is asleep, providing up to 1 mA for state retention.

The system features low-power states where unused logic is powered off, supporting wakeup from timer or IO events.

The amount of SRAM retained during power-down is configurable.

The internal 8 kB one-time-programmable storage (OTP) contains chip information such as unique identifiers, can be

used to configure hardware and bootrom security features, and can be programmed with user-supplied code and data.

The built-in bootrom implements direct boot from flash or OTP, and serial boot from USB or UART. Code signature

enforcement is supported for all boot media, using a key fingerprint registered in internal OTP storage. OTP can also

store decryption keys for encrypted boot, preventing flash contents from being read externally.

RISC-V architecture support is implemented by dynamically swapping the Cortex-M33 (Armv8-M) processors with

Hazard3 (RV32IMAC+) processors. Both architectures are available on all RP2350-family devices. The RISC-V cores

support debug over SWD, and can be programmed with the same SDK as the Arm cores.

1.2. Pinout reference

This section provides a quick reference for pinout and pin functions. Full details, including electrical specifications and

package drawings, can be found in Chapter 14.

1.2.1. Pin locations

1.2.1.1. QFN-60 (RP2350A)

Figure 2. RP2350

Pinout for QFN-60

7×7mm (reduced ePad

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

size)

1.2. Pinout reference
15
