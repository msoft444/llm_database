---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 1. Introduction
section: 1.2.1. Pin locations
pages: 16-16
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 1.2.1. Pin locations

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
VREG_AVDD
VREG_PGND
IOVDD
GPIO18
GPIO11
GPIO10
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
GPIO9
GPIO8
IOVDD
GPIO7
GPIO6
GPIO5
GPIO4
DVDD
GPIO3
GPIO2
GPIO1
GPIO0
IOVDD
GPIO20
GPIO21
GPIO22
GPIO23
GPIO24
GPIO25
IOVDD
DVDD
GPIO26_ADC0
GPIO27_ADC1
GPIO28_ADC2
GPIO29_ADC3
ADC_AVDD
IOVDD
VREG_LX
VREG_VIN
VREG_FB
USB_DM
USB_DP
USB_OTP_VDD
QSPI_IOVDD
QSPI_SD3
QSPI_SCLK
QSPI_SD0
QSPI_SD2
QSPI_SD1
QSPI_SS
45
60 59 58 57 56 55 54 53 52 51 50 49 48
16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
44
43
42
41
40
39
38
37
36
35
34
33
32
GPIO19
31
GPIO17
GPIO16
RUN
SWDIO
SWCLK
DVDD
XOUT
XIN
IOVDD
GPIO15
GPIO14
GPIO13
GPIO12
47 46
Figure 2. RP2350
Pinout for QFN-60
7×7mm (reduced ePad
size)
RP2350 Datasheet
1.2. Pinout reference
15

