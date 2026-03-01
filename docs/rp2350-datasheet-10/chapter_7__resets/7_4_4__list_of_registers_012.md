---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.4.4. List of registers
pages: 500-503
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 7.4.4. List of registers

7.4.4. List of registers

The PSM registers start at a base address of 0x40018000 (defined as PSM_BASE in SDK).

| Offset | Name | Info |
| --- | --- | --- |
| 0x0 | FRCE_ON | Force block out of reset (i.e. power it on) |
| 0x4 | FRCE_OFF | Force into reset (i.e. power it off) |
| 0x8 | WDSEL | Set to 1 if the watchdog should reset this |
| 0xc | DONE | Is the subsystem ready? |

*Table 529. List of PSM PSM: FRCE_ON Register Offset: 0x0 Description Force block out of reset (i.e. power it on)*

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:25 | Reserved. | - | - |
| 24 | PROC1 | RW | 0x0 |
| 23 | PROC0 | RW | 0x0 |
| 22 | ACCESSCTRL | RW | 0x0 |
| 21 | SIO | RW | 0x0 |
| 20 | XIP | RW | 0x0 |
| 19 | SRAM9 | RW | 0x0 |
| 18 | SRAM8 | RW | 0x0 |
| 17 | SRAM7 | RW | 0x0 |
| 16 | SRAM6 | RW | 0x0 |
| 15 | SRAM5 | RW | 0x0 |
| 14 | SRAM4 | RW | 0x0 |
| 13 | SRAM3 | RW | 0x0 |
| 12 | SRAM2 | RW | 0x0 |
| 11 | SRAM1 | RW | 0x0 |
| 10 | SRAM0 | RW | 0x0 |
| 9 | BOOTRAM | RW | 0x0 |
| 8 | ROM | RW | 0x0 |
| 7 | BUSFABRIC | RW | 0x0 |
| 6 | PSM_READY | RW | 0x0 |
| 5 | CLOCKS | RW | 0x0 |
| 4 | RESETS | RW | 0x0 |
| 3 | XOSC | RW | 0x0 |
| 2 | ROSC | RW | 0x0 |
| 1 | OTP | RW | 0x0 |
| 0 | PROC_COLD | RW | 0x0 |

*Table 530. FRCE_ON*

7.4. System resets (Power-on State Machine)
499

RP2350 Datasheet

PSM: FRCE_OFF Register

Offset: 0x4

Description

Force into reset (i.e. power it off)

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:25 | Reserved. | - | - |
| 24 | PROC1 | RW | 0x0 |
| 23 | PROC0 | RW | 0x0 |
| 22 | ACCESSCTRL | RW | 0x0 |
| 21 | SIO | RW | 0x0 |
| 20 | XIP | RW | 0x0 |
| 19 | SRAM9 | RW | 0x0 |
| 18 | SRAM8 | RW | 0x0 |
| 17 | SRAM7 | RW | 0x0 |
| 16 | SRAM6 | RW | 0x0 |
| 15 | SRAM5 | RW | 0x0 |
| 14 | SRAM4 | RW | 0x0 |
| 13 | SRAM3 | RW | 0x0 |
| 12 | SRAM2 | RW | 0x0 |
| 11 | SRAM1 | RW | 0x0 |
| 10 | SRAM0 | RW | 0x0 |
| 9 | BOOTRAM | RW | 0x0 |
| 8 | ROM | RW | 0x0 |
| 7 | BUSFABRIC | RW | 0x0 |
| 6 | PSM_READY | RW | 0x0 |
| 5 | CLOCKS | RW | 0x0 |
| 4 | RESETS | RW | 0x0 |
| 3 | XOSC | RW | 0x0 |
| 2 | ROSC | RW | 0x0 |
| 1 | OTP | RW | 0x0 |
| 0 | PROC_COLD | RW | 0x0 |

*Table 531. FRCE_OFF*

7.4. System resets (Power-on State Machine)
500

RP2350 Datasheet

PSM: WDSEL Register

Offset: 0x8

Description

Set to 1 if the watchdog should reset this

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:25 | Reserved. | - | - |
| 24 | PROC1 | RW | 0x0 |
| 23 | PROC0 | RW | 0x0 |
| 22 | ACCESSCTRL | RW | 0x0 |
| 21 | SIO | RW | 0x0 |
| 20 | XIP | RW | 0x0 |
| 19 | SRAM9 | RW | 0x0 |
| 18 | SRAM8 | RW | 0x0 |
| 17 | SRAM7 | RW | 0x0 |
| 16 | SRAM6 | RW | 0x0 |
| 15 | SRAM5 | RW | 0x0 |
| 14 | SRAM4 | RW | 0x0 |
| 13 | SRAM3 | RW | 0x0 |
| 12 | SRAM2 | RW | 0x0 |
| 11 | SRAM1 | RW | 0x0 |
| 10 | SRAM0 | RW | 0x0 |
| 9 | BOOTRAM | RW | 0x0 |
| 8 | ROM | RW | 0x0 |
| 7 | BUSFABRIC | RW | 0x0 |
| 6 | PSM_READY | RW | 0x0 |
| 5 | CLOCKS | RW | 0x0 |
| 4 | RESETS | RW | 0x0 |
| 3 | XOSC | RW | 0x0 |
| 2 | ROSC | RW | 0x0 |
| 1 | OTP | RW | 0x0 |
| 0 | PROC_COLD | RW | 0x0 |

*Table 532. WDSEL*

7.4. System resets (Power-on State Machine)
501

RP2350 Datasheet

PSM: DONE Register

Offset: 0xc

Description

Is the subsystem ready?

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:25 | Reserved. | - | - |
| 24 | PROC1 | RO | 0x0 |
| 23 | PROC0 | RO | 0x0 |
| 22 | ACCESSCTRL | RO | 0x0 |
| 21 | SIO | RO | 0x0 |
| 20 | XIP | RO | 0x0 |
| 19 | SRAM9 | RO | 0x0 |
| 18 | SRAM8 | RO | 0x0 |
| 17 | SRAM7 | RO | 0x0 |
| 16 | SRAM6 | RO | 0x0 |
| 15 | SRAM5 | RO | 0x0 |
| 14 | SRAM4 | RO | 0x0 |
| 13 | SRAM3 | RO | 0x0 |
| 12 | SRAM2 | RO | 0x0 |
| 11 | SRAM1 | RO | 0x0 |
| 10 | SRAM0 | RO | 0x0 |
| 9 | BOOTRAM | RO | 0x0 |
| 8 | ROM | RO | 0x0 |
| 7 | BUSFABRIC | RO | 0x0 |
| 6 | PSM_READY | RO | 0x0 |
| 5 | CLOCKS | RO | 0x0 |
| 4 | RESETS | RO | 0x0 |
| 3 | XOSC | RO | 0x0 |
| 2 | ROSC | RO | 0x0 |
| 1 | OTP | RO | 0x0 |
| 0 | PROC_COLD | RO | 0x0 |

*Table 533. DONE*

7.4. System resets (Power-on State Machine)
502
