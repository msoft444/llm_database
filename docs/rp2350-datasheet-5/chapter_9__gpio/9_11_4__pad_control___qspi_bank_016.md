---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.11.4. Pad Control - QSPI Bank
pages: 813-817
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 9.11.4. Pad Control - QSPI Bank

![Page 813 figure](images/fig_p0813.png)

9.11.4. Pad Control - QSPI Bank

The QSPI Bank Pad Control registers start at a base address of 0x40040000 (defined as PADS_QSPI_BASE in SDK).

Table 903. List of

PADS_QSPI registers
Offset
Name
Info

0x00
VOLTAGE_SELECT
Voltage select. Per bank control

PADS_QSPI: VOLTAGE_SELECT Register

Table 904.

Bits
Description
Type
Reset

VOLTAGE_SELECT

Register

31:1
Reserved.
-
-

0
Voltage select. Per bank control
RW
0x0

0x0 → 3V3: Set voltage to 3.3V (DVDD >= 2V5)

0x1 → 1V8: Set voltage to 1.8V (DVDD ⇐ 1V8)

9.11. List of registers
812

RP2350 Datasheet

PADS_QSPI: GPIO_QSPI_SCLK Register

Offset: 0x04

![Page 814 figure](images/fig_p0814.png)

Table 905.

Bits
Description
Type
Reset

GPIO_QSPI_SCLK

Register

31:9
Reserved.
-
-

8
ISO: Pad isolation control. Remove this once the pad is configured by

7
OD: Output disable. Has priority over output enable from peripherals
RW
0x0

6
IE: Input enable
RW
0x1

5:4
DRIVE: Drive strength.
RW
0x1

3
PUE: Pull up enable
RW
0x0

2
PDE: Pull down enable
RW
0x1

1
SCHMITT: Enable schmitt trigger
RW
0x1

0
SLEWFAST: Slew rate control. 1 = Fast, 0 = Slow
RW
0x0

PADS_QSPI: GPIO_QSPI_SD0 Register

Table 906.

Bits
Description
Type
Reset

GPIO_QSPI_SD0

Register

31:9
Reserved.
-
-

8
ISO: Pad isolation control. Remove this once the pad is configured by

7
OD: Output disable. Has priority over output enable from peripherals
RW
0x0

6
IE: Input enable
RW
0x1

5:4
DRIVE: Drive strength.
RW
0x1

3
PUE: Pull up enable
RW
0x0

2
PDE: Pull down enable
RW
0x1

1
SCHMITT: Enable schmitt trigger
RW
0x1

0
SLEWFAST: Slew rate control. 1 = Fast, 0 = Slow
RW
0x0

9.11. List of registers
813

RP2350 Datasheet

PADS_QSPI: GPIO_QSPI_SD1 Register

Offset: 0x0c

![Page 815 figure](images/fig_p0815.png)

Table 907.

Bits
Description
Type
Reset

GPIO_QSPI_SD1

Register

31:9
Reserved.
-
-

8
ISO: Pad isolation control. Remove this once the pad is configured by

7
OD: Output disable. Has priority over output enable from peripherals
RW
0x0

6
IE: Input enable
RW
0x1

5:4
DRIVE: Drive strength.
RW
0x1

3
PUE: Pull up enable
RW
0x0

2
PDE: Pull down enable
RW
0x1

1
SCHMITT: Enable schmitt trigger
RW
0x1

0
SLEWFAST: Slew rate control. 1 = Fast, 0 = Slow
RW
0x0

PADS_QSPI: GPIO_QSPI_SD2 Register

Table 908.

Bits
Description
Type
Reset

GPIO_QSPI_SD2

Register

31:9
Reserved.
-
-

8
ISO: Pad isolation control. Remove this once the pad is configured by

7
OD: Output disable. Has priority over output enable from peripherals
RW
0x0

6
IE: Input enable
RW
0x1

5:4
DRIVE: Drive strength.
RW
0x1

3
PUE: Pull up enable
RW
0x1

2
PDE: Pull down enable
RW
0x0

1
SCHMITT: Enable schmitt trigger
RW
0x1

0
SLEWFAST: Slew rate control. 1 = Fast, 0 = Slow
RW
0x0

9.11. List of registers
814

RP2350 Datasheet

PADS_QSPI: GPIO_QSPI_SD3 Register

Offset: 0x14

![Page 816 figure](images/fig_p0816.png)

Table 909.

Bits
Description
Type
Reset

GPIO_QSPI_SD3

Register

31:9
Reserved.
-
-

8
ISO: Pad isolation control. Remove this once the pad is configured by

7
OD: Output disable. Has priority over output enable from peripherals
RW
0x0

6
IE: Input enable
RW
0x1

5:4
DRIVE: Drive strength.
RW
0x1

3
PUE: Pull up enable
RW
0x1

2
PDE: Pull down enable
RW
0x0

1
SCHMITT: Enable schmitt trigger
RW
0x1

0
SLEWFAST: Slew rate control. 1 = Fast, 0 = Slow
RW
0x0

PADS_QSPI: GPIO_QSPI_SS Register

Table 910.

Bits
Description
Type
Reset

GPIO_QSPI_SS

Register

31:9
Reserved.
-
-

8
ISO: Pad isolation control. Remove this once the pad is configured by

7
OD: Output disable. Has priority over output enable from peripherals
RW
0x0

6
IE: Input enable
RW
0x1

5:4
DRIVE: Drive strength.
RW
0x1

3
PUE: Pull up enable
RW
0x1

2
PDE: Pull down enable
RW
0x0

1
SCHMITT: Enable schmitt trigger
RW
0x1

0
SLEWFAST: Slew rate control. 1 = Fast, 0 = Slow
RW
0x0

9.11. List of registers
815

RP2350 Datasheet

Chapter 10. Security

This chapter describes the RP2350 security model and the hardware that implements it. This chapter contains two

separate overviews: one for Arm, and one for RISC-V. The architectures have distinct security features and levels of

bootrom support.

10.1. Overview (Arm)

RP2350 provides hardware and bootrom security features for three purposes:

1. Prevent unauthorised code from running on the device

2. Prevent unauthorised reading of user code and data

3. Isolate trusted and untrusted software, running concurrently on the device, from one another

Point 1 is referred to in this datasheet as secure boot. Secure boot is a prerequisite to points two and three because

running unauthorised code on the device allows that code to access device internals. The bootrom secure boot

implementation and related hardware security features implement the root of trust for secure RP2350 applications;

bootrom contents are fixed at design time and immutable.

Point 2 is referred to in this datasheet as encrypted boot. Encrypted boot is an additional layer of protection which

makes it more difficult to clone devices, or dump and reverse-engineer device firmware. Encrypted boot is implemented

using a signed decryption stage prepended to a binary as a post-build step. Encrypted boot stores decryption keys in on-

device OTP memory, which can be locked down after use.

Point 3 allows applications to enforce internal security boundaries such that one part of an application being

compromised does not allow access to critical hardware, such as the voltage regulator or protected OTP storage used

for cryptographic keys.

Hardware features such as the glitch detector and redundancy coprocessor mitigate common classes of fault injection

attacks and help maintain boot integrity, even when an attacker has physical access.
