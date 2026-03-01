---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.3.11. List of registers
pages: 566-569
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 8.3.11. List of registers

![Page 566 figure](images/fig_p0566.png)

RP2350 Datasheet

WARNING

If you do not configure IRQ before entering DORMANT mode, neither oscillator will restart.

See Section 6.5.6.2, “DORMANT” for a some examples of dormant mode.

8.3.11. List of registers

The ROSC registers start at a base address of 0x400e8000 (defined as ROSC_BASE in SDK).

| Offset | Name | Info |
| --- | --- | --- |
| 0x00 | CTRL | Ring Oscillator control |
| 0x04 | FREQA | Ring Oscillator frequency control A |
| 0x08 | FREQB | Ring Oscillator frequency control B |
| 0x0c | RANDOM | Loads a value to the LFSR randomiser |
| 0x10 | DORMANT | Ring Oscillator pause control |
| 0x14 | DIV | Controls the output divider |
| 0x18 | PHASE | Controls the phase shifted output |
| 0x1c | STATUS | Ring Oscillator Status |
| 0x20 | RANDOMBIT | Returns a 1 bit random value |
| 0x24 | COUNT | A down counter running at the ROSC frequency which counts to zero and stops. |

Table 605. List of

ROSC: CTRL Register

Offset: 0x00

Description

Ring Oscillator control

Table 606. CTRL

Register
Bits
Description
Type
Reset

31:24
Reserved.
-
-

23:12
ENABLE: On power-up this field is initialised to ENABLE

RW
-

The system clock must be switched to another source before setting this field

to DISABLE otherwise the chip will lock up

The 12-bit code is intended to give some protection against accidental writes.

An invalid setting will enable the oscillator.

Enumerated values:

0xd1e → DISABLE

0xfab → ENABLE

8.3. Ring oscillator (ROSC)
565

![Page 567 figure](images/fig_p0567.png)

RP2350 Datasheet

Bits
Description
Type
Reset

11:0
FREQ_RANGE: Controls the number of delay stages in the ROSC ring

RW
0xaa0

LOW uses stages 0 to 7

MEDIUM uses stages 0 to 5

HIGH uses stages 0 to 3

TOOHIGH uses stages 0 to 1 and should not be used because its frequency

exceeds design specifications

The clock output will not glitch when changing the range up one step at a time

The clock output will glitch when changing the range down

Note: the values here are gray coded which is why HIGH comes before

TOOHIGH

Enumerated values:

0xfa4 → LOW

0xfa5 → MEDIUM

0xfa7 → HIGH

0xfa6 → TOOHIGH

ROSC: FREQA Register

Offset: 0x04

Description

The FREQA & FREQB registers control the frequency by controlling the drive strength of each stage

The drive strength has 4 levels determined by the number of bits set

Increasing the number of bits set increases the drive strength and increases the oscillation frequency

0 bits set is the default drive strength

1 bit set doubles the drive strength

2 bits set triples drive strength

3 bits set quadruples drive strength

For frequency randomisation set both DS0_RANDOM=1 & DS1_RANDOM=1

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:16 | PASSWD: Set to 0x9696 to apply the settings Any other value in this field will set all drive strengths to 0 | RW | 0x0000 |
|  | Enumerated values: |  |  |
|  | 0x9696 → PASS |  |  |
| 15 | Reserved. | - | - |
| 14:12 | DS3: Stage 3 drive strength | RW | 0x0 |
| 11 | Reserved. | - | - |
| 10:8 | DS2: Stage 2 drive strength | RW | 0x0 |
| 7 | DS1_RANDOM: Randomises the stage 1 drive strength | RW | 0x1 |
| 6:4 | DS1: Stage 1 drive strength | RW | 0x0 |
| 3 | DS0_RANDOM: Randomises the stage 0 drive strength | RW | 0x1 |
| 2:0 | DS0: Stage 0 drive strength | RW | 0x0 |

Table 607. FREQA

ROSC: FREQB Register

Offset: 0x08

8.3. Ring oscillator (ROSC)
566

![Page 568 figure](images/fig_p0568.png)

RP2350 Datasheet

Description

For a detailed description see freqa register

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:16 | PASSWD: Set to 0x9696 to apply the settings Any other value in this field will set all drive strengths to 0 | RW | 0x0000 |
|  | Enumerated values: |  |  |
|  | 0x9696 → PASS |  |  |
| 15 | Reserved. | - | - |
| 14:12 | DS7: Stage 7 drive strength | RW | 0x0 |
| 11 | Reserved. | - | - |
| 10:8 | DS6: Stage 6 drive strength | RW | 0x0 |
| 7 | Reserved. | - | - |
| 6:4 | DS5: Stage 5 drive strength | RW | 0x0 |
| 3 | Reserved. | - | - |
| 2:0 | DS4: Stage 4 drive strength | RW | 0x0 |

Table 608. FREQB

ROSC: RANDOM Register

Offset: 0x0c

Description

Loads a value to the LFSR randomiser

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | SEED | RW | 0x3f04b16d |

Table 609. RANDOM

ROSC: DORMANT Register

Offset: 0x10

Description

Ring Oscillator pause control

Table 610. DORMANT

Register
Bits
Description
Type
Reset

31:0
This is used to save power by pausing the ROSC

RW
-

On power-up this field is initialised to WAKE

An invalid write will also select WAKE

Warning: setup the irq before selecting dormant mode

Enumerated values:

0x636f6d61 → DORMANT

0x77616b65 → WAKE

ROSC: DIV Register

Offset: 0x14

Description

Controls the output divider

8.3. Ring oscillator (ROSC)
567

![Page 569 figure](images/fig_p0569.png)

RP2350 Datasheet

Table 611. DIV

Register
Bits
Description
Type
Reset

31:16
Reserved.
-
-

15:0
set to 0xaa00 + div where

RW
-

div = 0 divides by 128

div = 1-127 divides by div

any other value sets div=128

this register resets to div=32

Enumerated values:

0xaa00 → PASS

ROSC: PHASE Register

Offset: 0x18

Description

Controls the phase shifted output

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:12 | Reserved. | - | - |
| 11:4 | PASSWD: set to 0xaa any other value enables the output with shift=0 | RW | 0x00 |
| 3 | ENABLE: enable the phase-shifted output this can be changed on-the-fly | RW | 0x1 |
| 2 | FLIP: invert the phase-shifted output this is ignored when div=1 | RW | 0x0 |
| 1:0 | SHIFT: phase shift the phase-shifted output by SHIFT input clocks this can be changed on-the-fly must be set to 0 before setting div=1 | RW | 0x0 |

Table 612. PHASE

ROSC: STATUS Register

Offset: 0x1c

Description

Ring Oscillator Status

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31 | STABLE: Oscillator is running and stable | RO | 0x0 |
| 30:25 | Reserved. | - | - |
| 24 | BADWRITE: An invalid value has been written to CTRL_ENABLE or CTRL_FREQ_RANGE or FREQA or FREQB or DIV or PHASE or DORMANT | WC | 0x0 |
| 23:17 | Reserved. | - | - |
| 16 | DIV_RUNNING: post-divider is running this resets to 0 but transitions to 1 during chip startup | RO | - |
| 15:13 | Reserved. | - | - |
| 12 | ENABLED: Oscillator is enabled but not necessarily running and stable this resets to 0 but transitions to 1 during chip startup | RO | - |
| 11:0 | Reserved. | - | - |

Table 613. STATUS

8.3. Ring oscillator (ROSC)
568
