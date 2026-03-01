---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.2.8. List of registers
pages: 560-561
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 8.2.8. List of registers

![Page 560 figure](images/fig_p0560.png)

The XOSC registers start at a base address of 0x40048000 (defined as XOSC_BASE in SDK).

| Offset | Name | Info |
| --- | --- | --- |
| 0x00 | CTRL | Crystal Oscillator Control |
| 0x04 | STATUS | Crystal Oscillator Status |
| 0x08 | DORMANT | Crystal Oscillator pause control |
| 0x0c | STARTUP | Controls the startup delay |
| 0x10 | COUNT | A down counter running at the XOSC frequency which counts to zero and stops. |

Table 598. List of

Table 599. CTRL

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
ENABLE: On power-up this field is initialised to DISABLE and the chip runs

If the chip has subsequently been programmed to run from the XOSC then

setting this field to DISABLE may lock-up the chip. If this is a concern then run

the clk_ref from the ROSC and enable the clk_sys RESUS feature.

The 12-bit code is intended to give some protection against accidental writes.

An invalid setting will retain the previous value. The actual value being used

can be read from STATUS_ENABLED

11:0
FREQ_RANGE: The 12-bit code is intended to give some protection against

accidental writes. An invalid setting will retain the previous value. The actual

value being used can be read from STATUS_FREQ_RANGE

8.2. Crystal oscillator (XOSC)
559

RP2350 Datasheet

XOSC: STATUS Register

Offset: 0x04

Description

Crystal Oscillator Status

![Page 561 figure](images/fig_p0561.png)

Table 600. STATUS

Register
Bits
Description
Type
Reset

31
STABLE: Oscillator is running and stable
RO
0x0

30:25
Reserved.
-
-

24
BADWRITE: An invalid value has been written to CTRL_ENABLE or

23:13
Reserved.
-
-

12
ENABLED: Oscillator is enabled but not necessarily running and stable, resets

11:2
Reserved.
-
-

1:0
FREQ_RANGE: The current frequency range setting
RO
-

Crystal Oscillator pause control

Table 601. DORMANT

Register
Bits
Description
Type
Reset

On power-up this field is initialised to WAKE

An invalid write will also select WAKE

WARNING: stop the PLLs before selecting dormant mode

WARNING: setup the irq before selecting dormant mode

XOSC: STARTUP Register

Offset: 0x0c

Description

Controls the startup delay

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:21 | Reserved. | - | - |

Table 602. STARTUP

8.2. Crystal oscillator (XOSC)
560
