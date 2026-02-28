---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.11.4. Pad Control - QSPI Bank
pages: 813-816
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 9.11.4. Pad Control - QSPI Bank

Table 902. SWD
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8
ISO: Pad isolation control. Remove this once the pad is configured by
software.
RW
0x0
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
Enumerated values:
0x0 → 2MA
0x1 → 4MA
0x2 → 8MA
0x3 → 12MA
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
0x04
GPIO_QSPI_SCLK
0x08
GPIO_QSPI_SD0
0x0c
GPIO_QSPI_SD1
0x10
GPIO_QSPI_SD2
0x14
GPIO_QSPI_SD3
0x18
GPIO_QSPI_SS
PADS_QSPI: VOLTAGE_SELECT Register
Offset: 0x00
Table 904.
VOLTAGE_SELECT
Register
Bits
Description
Type
Reset
31:1
Reserved.
-
-
0
Voltage select. Per bank control
RW
0x0
Enumerated values:
0x0 → 3V3: Set voltage to 3.3V (DVDD >= 2V5)
0x1 → 1V8: Set voltage to 1.8V (DVDD ⇐ 1V8)
RP2350 Datasheet
9.11. List of registers
812


PADS_QSPI: GPIO_QSPI_SCLK Register
Offset: 0x04
Table 905.
GPIO_QSPI_SCLK
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8
ISO: Pad isolation control. Remove this once the pad is configured by
software.
RW
0x1
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
Enumerated values:
0x0 → 2MA
0x1 → 4MA
0x2 → 8MA
0x3 → 12MA
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
Offset: 0x08
Table 906.
GPIO_QSPI_SD0
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8
ISO: Pad isolation control. Remove this once the pad is configured by
software.
RW
0x1
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
Enumerated values:
0x0 → 2MA
0x1 → 4MA
0x2 → 8MA
0x3 → 12MA
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
RP2350 Datasheet
9.11. List of registers
813


PADS_QSPI: GPIO_QSPI_SD1 Register
Offset: 0x0c
Table 907.
GPIO_QSPI_SD1
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8
ISO: Pad isolation control. Remove this once the pad is configured by
software.
RW
0x1
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
Enumerated values:
0x0 → 2MA
0x1 → 4MA
0x2 → 8MA
0x3 → 12MA
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
Offset: 0x10
Table 908.
GPIO_QSPI_SD2
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8
ISO: Pad isolation control. Remove this once the pad is configured by
software.
RW
0x1
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
Enumerated values:
0x0 → 2MA
0x1 → 4MA
0x2 → 8MA
0x3 → 12MA
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
RP2350 Datasheet
9.11. List of registers
814


PADS_QSPI: GPIO_QSPI_SD3 Register
Offset: 0x14
Table 909.
GPIO_QSPI_SD3
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8
ISO: Pad isolation control. Remove this once the pad is configured by
software.
RW
0x1
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
Enumerated values:
0x0 → 2MA
0x1 → 4MA
0x2 → 8MA
0x3 → 12MA
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
Offset: 0x18
Table 910.
GPIO_QSPI_SS
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8
ISO: Pad isolation control. Remove this once the pad is configured by
software.
RW
0x1
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
Enumerated values:
0x0 → 2MA
0x1 → 4MA
0x2 → 8MA
0x3 → 12MA
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
RP2350 Datasheet
9.11. List of registers
815

