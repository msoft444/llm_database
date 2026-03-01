---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.8.5. List of registers
pages: 1189-1194
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 12.8.5. List of registers

12.8.5. List of registers

The TIMER0 and TIMER1 registers start at base addresses of 0x400b0000 and 0x400b8000 respectively (defined as

TIMER0_BASE and TIMER1_BASE in SDK).

| Offset | Name | Info |
| --- | --- | --- |
| 0x00 | TIMEHW | Write to bits 63:32 of time always write timelw before timehw |
| 0x04 | TIMELW | Write to bits 31:0 of time writes do not get copied to time until timehw is written |
| 0x08 | TIMEHR | Read from bits 63:32 of time always read timelr before timehr |
| 0x0c | TIMELR | Read from bits 31:0 of time |
| 0x10 | ALARM0 | Arm alarm 0, and configure the time it will fire. Once armed, the alarm fires when TIMER_ALARM0 == TIMELR. The alarm will disarm itself once it fires, and can be disarmed early using the ARMED status register. |
| 0x14 | ALARM1 | Arm alarm 1, and configure the time it will fire. Once armed, the alarm fires when TIMER_ALARM1 == TIMELR. The alarm will disarm itself once it fires, and can be disarmed early using the ARMED status register. |
| 0x18 | ALARM2 | Arm alarm 2, and configure the time it will fire. Once armed, the alarm fires when TIMER_ALARM2 == TIMELR. The alarm will disarm itself once it fires, and can be disarmed early using the ARMED status register. |
| 0x1c | ALARM3 | Arm alarm 3, and configure the time it will fire. Once armed, the alarm fires when TIMER_ALARM3 == TIMELR. The alarm will disarm itself once it fires, and can be disarmed early using the ARMED status register. |
| 0x20 | ARMED | Indicates the armed/disarmed status of each alarm. A write to the corresponding ALARMx register arms the alarm. Alarms automatically disarm upon firing, but writing ones here will disarm immediately without waiting to fire. |
| 0x24 | TIMERAWH | Raw read from bits 63:32 of time (no side effects) |
| 0x28 | TIMERAWL | Raw read from bits 31:0 of time (no side effects) |
| 0x2c | DBGPAUSE | Set bits high to enable pause when the corresponding debug ports are active |
| 0x30 | PAUSE | Set high to pause the timer |
| 0x34 | LOCKED | Set locked bit to disable write access to timer Once set, cannot be cleared (without a reset) |
| 0x38 | SOURCE | Selects the source for the timer. Defaults to the normal tick configured in the ticks block (typically configured to 1 microsecond). Writing to 1 will ignore the tick and count clk_sys cycles instead. |
| 0x3c | INTR | Raw Interrupts |
| 0x40 | INTE | Interrupt Enable |
| 0x44 | INTF | Interrupt Force |
| 0x48 | INTS | Interrupt status after masking & forcing |

Table 1227. List of

12.8. System timers
1188

RP2350 Datasheet

TIMER: TIMEHW Register

Offset: 0x00

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Write to bits 63:32 of time always write timelw before timehw | WF | 0x00000000 |

Table 1228. TIMEHW

TIMER: TIMELW Register

Offset: 0x04

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Write to bits 31:0 of time writes do not get copied to time until timehw is written | WF | 0x00000000 |

Table 1229. TIMELW

TIMER: TIMEHR Register

Offset: 0x08

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Read from bits 63:32 of time always read timelr before timehr | RO | 0x00000000 |

Table 1230. TIMEHR

TIMER: TIMELR Register

Offset: 0x0c

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Read from bits 31:0 of time | RO | 0x00000000 |
| 31:0 | Arm alarm 0, and configure the time it will fire. Once armed, the alarm fires when TIMER_ALARM0 == TIMELR. The alarm will disarm itself once it fires, and can be disarmed early using the ARMED status register. | RW | 0x00000000 |

Table 1231. TIMELR

TIMER: ALARM0 Register

Offset: 0x10

12.8. System timers
1189

RP2350 Datasheet

Table 1232. ALARM0

TIMER: ALARM1 Register

Offset: 0x14

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Arm alarm 1, and configure the time it will fire. Once armed, the alarm fires when TIMER_ALARM1 == TIMELR. The alarm will disarm itself once it fires, and can be disarmed early using the ARMED status register. | RW | 0x00000000 |

Table 1233. ALARM1

TIMER: ALARM2 Register

Offset: 0x18

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Arm alarm 2, and configure the time it will fire. Once armed, the alarm fires when TIMER_ALARM2 == TIMELR. The alarm will disarm itself once it fires, and can be disarmed early using the ARMED status register. | RW | 0x00000000 |

Table 1234. ALARM2

TIMER: ALARM3 Register

Offset: 0x1c

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Arm alarm 3, and configure the time it will fire. Once armed, the alarm fires when TIMER_ALARM3 == TIMELR. The alarm will disarm itself once it fires, and can be disarmed early using the ARMED status register. | RW | 0x00000000 |

Table 1235. ALARM3

TIMER: ARMED Register

Offset: 0x20

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:4 | Reserved. | - | - |
| 3:0 | Indicates the armed/disarmed status of each alarm. A write to the corresponding ALARMx register arms the alarm. Alarms automatically disarm upon firing, but writing ones here will disarm immediately without waiting to fire. | WC | 0x0 |
| 31:0 | Raw read from bits 63:32 of time (no side effects) | RO | 0x00000000 |

Table 1236. ARMED

TIMER: TIMERAWH Register

Offset: 0x24

12.8. System timers
1190

RP2350 Datasheet

Table 1237.

TIMER: TIMERAWL Register

Offset: 0x28

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Raw read from bits 31:0 of time (no side effects) | RO | 0x00000000 |

Table 1238.

TIMER: DBGPAUSE Register

Offset: 0x2c

Description

Set bits high to enable pause when the corresponding debug ports are active

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:3 | Reserved. | - | - |
| 2 | DBG1: Pause when processor 1 is in debug mode | RW | 0x1 |
| 1 | DBG0: Pause when processor 0 is in debug mode | RW | 0x1 |
| 0 | Reserved. | - | - |

Table 1239.

TIMER: PAUSE Register

Offset: 0x30

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:1 | Reserved. | - | - |
| 0 | Set high to pause the timer | RW | 0x0 |

Table 1240. PAUSE

TIMER: LOCKED Register

Offset: 0x34

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:1 | Reserved. | - | - |
| 0 | Set locked bit to disable write access to timer Once set, cannot be cleared (without a reset) | RW | 0x0 |

Table 1241. LOCKED

TIMER: SOURCE Register

Offset: 0x38

Description

Selects the source for the timer. Defaults to the normal tick configured in the ticks block (typically configured to 1

microsecond). Writing to 1 will ignore the tick and count clk_sys cycles instead.

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:1 | Reserved. | - | - |
| 0 | CLK_SYS | RW | 0x0 |

Table 1242. SOURCE

12.8. System timers
1191

RP2350 Datasheet

![Page 1193 figure](images/fig_p1193.png)

Bits
Description
Type
Reset

TIMER: INTR Register

Offset: 0x3c

Description

Raw Interrupts

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:4 | Reserved. | - | - |
| 3 | ALARM_3 | WC | 0x0 |
| 2 | ALARM_2 | WC | 0x0 |
| 1 | ALARM_1 | WC | 0x0 |
| 0 | ALARM_0 | WC | 0x0 |

Table 1243. INTR

TIMER: INTE Register

Offset: 0x40

Description

Interrupt Enable

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:4 | Reserved. | - | - |
| 3 | ALARM_3 | RW | 0x0 |
| 2 | ALARM_2 | RW | 0x0 |
| 1 | ALARM_1 | RW | 0x0 |
| 0 | ALARM_0 | RW | 0x0 |

Table 1244. INTE

TIMER: INTF Register

Offset: 0x44

Description

Interrupt Force

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:4 | Reserved. | - | - |
| 3 | ALARM_3 | RW | 0x0 |
| 2 | ALARM_2 | RW | 0x0 |
| 1 | ALARM_1 | RW | 0x0 |
| 0 | ALARM_0 | RW | 0x0 |
| 31:4 | Reserved. | - | - |
| 3 | ALARM_3 | RO | 0x0 |
| 2 | ALARM_2 | RO | 0x0 |
| 1 | ALARM_1 | RO | 0x0 |
| 0 | ALARM_0 | RO | 0x0 |

Table 1245. INTF

TIMER: INTS Register

12.8. System timers
1192

RP2350 Datasheet

Offset: 0x48

Description

Interrupt status after masking & forcing

Table 1246. INTS

12.9. Watchdog
