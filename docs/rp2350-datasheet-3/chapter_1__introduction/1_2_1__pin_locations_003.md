---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 1. Introduction
section: 1.2.1. Pin locations
pages: 16-16
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 1.2.1. Pin locations

![Page 16 figure](images/fig_p0016.png)

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

USB_OTP_VDD

size)

VREG_PGND

QSPI_IOVDD

VREG_AVDD

QSPI_SCLK

VREG_VIN

QSPI_SD3

QSPI_SD0

QSPI_SD2

QSPI_SD1

VREG_FB

VREG_LX

QSPI_SS

USB_DM

USB_DP

60 59 58 57 56 55 54 53 52 51 50 49 48

47 46

1

45

IOVDD

IOVDD

2

44

GPIO0

ADC_AVDD

3

43

GPIO1

GPIO29_ADC3

4

42

GPIO2

GPIO28_ADC2

5

41

GPIO3

GPIO27_ADC1

6

40

DVDD

GPIO26_ADC0

7

39

GPIO4

DVDD

GND

8

38

GPIO5

IOVDD

9

37

GPIO6

GPIO25

10

36

GPIO7

GPIO24

11

35

IOVDD

GPIO23

12

34

GPIO8

GPIO22

13

33

GPIO9

GPIO21

TOP VIEW

GPIO10

14

GPIO20

32

15

GPIO11

GPIO19
31

16 17 18 19 20 21 22 23 24 25 26 27 28 29 30

RUN

XIN

XOUT

GPIO18

GPIO17

GPIO16

GPIO15

GPIO14

GPIO13

GPIO12

SWCLK

DVDD

IOVDD

IOVDD

SWDIO

1.2. Pinout reference
15
