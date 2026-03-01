---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.5.2. List of registers
pages: 572-576
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 8.5.2. List of registers

8.5.2. List of registers

The tick generator registers start at a base address of 0x40108000 (defined as TICKS_BASE in SDK).

![Page 572 figure](images/fig_p0572.png)

Table 617. List of

TICKS registers
Offset
Name
Info

0x00
PROC0_CTRL
Controls the tick generator

0x0c
PROC1_CTRL
Controls the tick generator

0x18
TIMER0_CTRL
Controls the tick generator

0x24
TIMER1_CTRL
Controls the tick generator

0x30
WATCHDOG_CTRL
Controls the tick generator

0x3c
RISCV_CTRL
Controls the tick generator

TICKS: PROC0_CTRL Register

Offset: 0x00

Description

Controls the tick generator

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:2 | Reserved. | - | - |
| 1 | RUNNING: Is the tick generator running? | RO | - |
| 0 | ENABLE: start / stop tick generation | RW | 0x0 |
| 31:9 | Reserved. | - | - |
| 8:0 | Total number of clk_tick cycles before the next tick. | RW | 0x000 |

Table 618.

TICKS: PROC0_CYCLES Register

Offset: 0x04

8.5. Tick generators
571

RP2350 Datasheet

Table 619.

PROC0_CYCLES

Register

TICKS: PROC0_COUNT Register

Offset: 0x08

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:9 | Reserved. | - | - |
| 8:0 | Count down timer: the remaining number clk_tick cycles before the next tick is generated. | RO | - |

Table 620.

PROC0_COUNT

Register

TICKS: PROC1_CTRL Register

Offset: 0x0c

Description

Controls the tick generator

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:2 | Reserved. | - | - |
| 1 | RUNNING: Is the tick generator running? | RO | - |
| 0 | ENABLE: start / stop tick generation | RW | 0x0 |

Table 621.

TICKS: PROC1_CYCLES Register

Offset: 0x10

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:9 | Reserved. | - | - |
| 8:0 | Total number of clk_tick cycles before the next tick. | RW | 0x000 |

Table 622.

PROC1_CYCLES

Register

TICKS: PROC1_COUNT Register

Offset: 0x14

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:9 | Reserved. | - | - |
| 8:0 | Count down timer: the remaining number clk_tick cycles before the next tick is generated. | RO | - |

Table 623.

PROC1_COUNT

Register

TICKS: TIMER0_CTRL Register

Offset: 0x18

Description

Controls the tick generator

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:2 | Reserved. | - | - |
| 1 | RUNNING: Is the tick generator running? | RO | - |
| 0 | ENABLE: start / stop tick generation | RW | 0x0 |

Table 624.

8.5. Tick generators
572

RP2350 Datasheet

TICKS: TIMER0_CYCLES Register

Offset: 0x1c

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:9 | Reserved. | - | - |
| 8:0 | Total number of clk_tick cycles before the next tick. | RW | 0x000 |

Table 625.

TIMER0_CYCLES

Register

TICKS: TIMER0_COUNT Register

Offset: 0x20

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:9 | Reserved. | - | - |
| 8:0 | Count down timer: the remaining number clk_tick cycles before the next tick is generated. | RO | - |

Table 626.

TIMER0_COUNT

Register

TICKS: TIMER1_CTRL Register

Offset: 0x24

Description

Controls the tick generator

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:2 | Reserved. | - | - |
| 1 | RUNNING: Is the tick generator running? | RO | - |
| 0 | ENABLE: start / stop tick generation | RW | 0x0 |

Table 627.

TICKS: TIMER1_CYCLES Register

Offset: 0x28

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:9 | Reserved. | - | - |
| 8:0 | Total number of clk_tick cycles before the next tick. | RW | 0x000 |
| 31:9 | Reserved. | - | - |
| 8:0 | Count down timer: the remaining number clk_tick cycles before the next tick is generated. | RO | - |

Table 628.

TIMER1_CYCLES

Register

TICKS: TIMER1_COUNT Register

Offset: 0x2c

8.5. Tick generators
573

RP2350 Datasheet

Table 629.

TIMER1_COUNT

Register

TICKS: WATCHDOG_CTRL Register

Offset: 0x30

Description

Controls the tick generator

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:2 | Reserved. | - | - |
| 1 | RUNNING: Is the tick generator running? | RO | - |
| 0 | ENABLE: start / stop tick generation | RW | 0x0 |

Table 630.

WATCHDOG_CTRL

Register

TICKS: WATCHDOG_CYCLES Register

Offset: 0x34

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:9 | Reserved. | - | - |
| 8:0 | Total number of clk_tick cycles before the next tick. | RW | 0x000 |

Table 631.

WATCHDOG_CYCLES

Register

TICKS: WATCHDOG_COUNT Register

Offset: 0x38

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:9 | Reserved. | - | - |
| 8:0 | Count down timer: the remaining number clk_tick cycles before the next tick is generated. | RO | - |

Table 632.

WATCHDOG_COUNT

Register

TICKS: RISCV_CTRL Register

Offset: 0x3c

Description

Controls the tick generator

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:2 | Reserved. | - | - |
| 1 | RUNNING: Is the tick generator running? | RO | - |
| 0 | ENABLE: start / stop tick generation | RW | 0x0 |
| 31:9 | Reserved. | - | - |
| 8:0 | Total number of clk_tick cycles before the next tick. | RW | 0x000 |

Table 633.

TICKS: RISCV_CYCLES Register

Offset: 0x40

8.5. Tick generators
574

RP2350 Datasheet

Table 634.

RISCV_CYCLES

Register

TICKS: RISCV_COUNT Register

Offset: 0x44

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:9 | Reserved. | - | - |
| 8:0 | Count down timer: the remaining number clk_tick cycles before the next tick is generated. | RO | - |

Table 635.

RISCV_COUNT

Register

8.6. PLL

![Page 576 figure](images/fig_p0576.png)
