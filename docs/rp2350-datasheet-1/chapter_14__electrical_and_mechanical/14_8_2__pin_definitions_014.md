---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 14. Electrical and mechanical
section: 14.8.2. Pin definitions
pages: 1336-1338
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 14.8.2. Pin definitions

14.8.2. Pin definitions
14.8.2.1. Pin types
In the following pin tables (Table 1427), the pin types are defined as shown below.
Table 1426. Pin Types
Pin Type
Direction
Description
Digital In
Input only
Standard Digital. Programmable Pull-Up, Pull-Down, Slew Rate,
Schmitt Trigger and Drive Strength. Default Drive Strength is 4 mA.
Digital IO
Bi-directional
Digital In (FT)
Input only
Fault Tolerant Digital. These pins are described as Fault Tolerant,
which in this case means that very little current flows into the pin
whilst it is below 3.63 V and IOVDD is 0 V. Additionally, they will
tolerate voltages up to 5.5 V, provided IOVDD is powered to 3.3 V.
These pins have enhanced ESD protection. Programmable Pull-Up,
Pull-Down, Slew Rate, Schmitt Trigger and Drive Strength. Default Drive
Strength is 4 mA.
Digital IO (FT)
Bi-directional
Digital IO / Analogue
Bi-directional (digital),
Input (Analogue)
Standard Digital and ADC input. Programmable Pull-Up, Pull-Down,
Slew Rate, Schmitt Trigger and Drive Strength. Default Drive Strength
is 4 mA.
USB IO
Bi-directional
These pins are for USB use, and contain internal pull-up and pull-down
resistors, as per the USB specification. USB operation requires
external 27Ω series resistors.
Analogue (XOSC)
Oscillator input pins for attaching a 12 MHz crystal. Alternatively, XIN
may be driven by a square wave.
14.8.2.2. Pin list
Table 1427. GPIO pins
Name
QFN-60 Number
QFN-80 Number
Type
Power Domain
Reset State
Description
GPIO0
2
77
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO1
3
78
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO2
4
79
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO3
5
80
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO4
7
1
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO5
8
2
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO6
9
3
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO7
10
4
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO8
12
6
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO9
13
7
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO10
14
8
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO11
15
9
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO12
16
11
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO13
17
12
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO14
18
13
Digital IO (FT)
IOVDD
Pull-Down
User IO
RP2350 Datasheet
14.8. Pinout
1335


Name
QFN-60 Number
QFN-80 Number
Type
Power Domain
Reset State
Description
GPIO15
19
14
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO16
27
16
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO17
28
17
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO18
29
18
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO19
31
19
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO20
32
20
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO21
33
21
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO22
34
22
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO23
35
23
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO24
36
25
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO25
37
26
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO26_ADC0
40
-
Digital IO /
Analogue
IOVDD /
ADC_AVDD
Pull-Down
User IO or ADC
input
GPIO27_ADC1
41
-
Digital IO /
Analogue
IOVDD /
ADC_AVDD
Pull-Down
User IO or ADC
input
GPIO28_ADC2
42
-
Digital IO /
Analogue
IOVDD /
ADC_AVDD
Pull-Down
User IO or ADC
input
GPIO29_ADC3
43
-
Digital IO /
Analogue
IOVDD /
ADC_AVDD
Pull-Down
User IO or ADC
input
GPIO26
-
27
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO27
-
28
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO28
-
36
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO29
-
37
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO30
-
38
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO31
-
39
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO32
-
40
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO33
-
42
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO34
-
43
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO35
-
44
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO36
-
45
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO37
-
46
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO38
-
47
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO39
-
48
Digital IO (FT)
IOVDD
Pull-Down
User IO
GPIO40_ADC0
-
49
Digital IO /
Analogue
IOVDD /
ADC_AVDD
Pull-Down
User IO or ADC
input
GPIO41_ADC1
-
52
Digital IO /
Analogue
IOVDD /
ADC_AVDD
Pull-Down
User IO or ADC
input
RP2350 Datasheet
14.8. Pinout
1336


Name
QFN-60 Number
QFN-80 Number
Type
Power Domain
Reset State
Description
GPIO42_ADC2
-
53
Digital IO /
Analogue
IOVDD /
ADC_AVDD
Pull-Down
User IO or ADC
input
GPIO43_ADC3
-
54
Digital IO /
Analogue
IOVDD /
ADC_AVDD
Pull-Down
User IO or ADC
input
GPIO44_ADC4
-
55
Digital IO /
Analogue
IOVDD /
ADC_AVDD
Pull-Down
User IO or ADC
input
GPIO45_ADC5
-
56
Digital IO /
Analogue
IOVDD /
ADC_AVDD
Pull-Down
User IO or ADC
input
GPIO46_ADC6
-
57
Digital IO /
Analogue
IOVDD /
ADC_AVDD
Pull-Down
User IO or ADC
input
GPIO47_ADC7
-
58
Digital IO /
Analogue
IOVDD /
ADC_AVDD
Pull-Down
User IO or ADC
input
Table 1428. QSPI pins
Name
QFN-60 Number
QFN-80 Number
Type
Power Domain
Reset State
Description
QSPI_SD3
55
70
Digital IO
QSPI_IOVDD
Pull-Up
QSPI data
QSPI_SCLK
56
71
Digital IO
QSPI_IOVDD
Pull-Down
QSPI clock
QSPI_SD0
57
72
Digital IO
QSPI_IOVDD
Pull-Down
QSPI data
QSPI_SD2
58
73
Digital IO
QSPI_IOVDD
Pull-Up
QSPI data
QSPI_SD1
59
74
Digital IO
QSPI_IOVDD
Pull-Down
QSPI data
QSPI_SS
60
75
Digital IO
QSPI_IOVDD
Pull-Up
QSPI chip
select / USB
BOOTSEL
Table 1429. Crystal
oscillator pins
Name
QFN-60 Number
QFN-80 Number
Type
Power Domain
Description
XIN
21
30
Analogue (XOSC)
IOVDD
Crystal oscillator.
XIN may also be
driven by a square
wave.
XOUT
22
31
Analogue (XOSC)
IOVDD
Crystal oscillator.
Table 1430.
Miscellaneous pins
Name
QFN-60 Number
QFN-80 Number
Type
Power Domain
Reset State
Description
RUN
26
35
Digital In (FT)
IOVDD
Pull-Up
Chip enable /
reset_n
SWCLK
24
33
Digital In (FT)
IOVDD
Pull-Up
Serial Wire
Debug clock
SWDIO
25
34
Digital IO (FT)
IOVDD
Pull-Up
Serial Wire
Debug data
Table 1431. USB pins
Name
QFN-60 Number
QFN-80 Number
Type
Power Domain
Description
USB_DP
52
67
USB IO
USB_OTP_VDD
USB Data +ve.
27Ω series
resistor required
for USB operation
RP2350 Datasheet
14.8. Pinout
1337

