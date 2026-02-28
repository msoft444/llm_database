---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.5.2. List of registers
pages: 572-575
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 8.5.2. List of registers

the hardware needs to count for 12 times as many clock cycles to get a 1 μs tick from a reference running at 12 ×
1 MHz.
Before changing the cycle count, always stop the tick generator with the TIMER0_CTRL.ENABLE bit. You can re-enable
once the tick generator is configured.
8.5.2. List of registers
The tick generator registers start at a base address of 0x40108000 (defined as TICKS_BASE in SDK).
Table 617. List of
TICKS registers
Offset
Name
Info
0x00
PROC0_CTRL
Controls the tick generator
0x04
PROC0_CYCLES
0x08
PROC0_COUNT
0x0c
PROC1_CTRL
Controls the tick generator
0x10
PROC1_CYCLES
0x14
PROC1_COUNT
0x18
TIMER0_CTRL
Controls the tick generator
0x1c
TIMER0_CYCLES
0x20
TIMER0_COUNT
0x24
TIMER1_CTRL
Controls the tick generator
0x28
TIMER1_CYCLES
0x2c
TIMER1_COUNT
0x30
WATCHDOG_CTRL
Controls the tick generator
0x34
WATCHDOG_CYCLES
0x38
WATCHDOG_COUNT
0x3c
RISCV_CTRL
Controls the tick generator
0x40
RISCV_CYCLES
0x44
RISCV_COUNT
TICKS: PROC0_CTRL Register
Offset: 0x00
Description
Controls the tick generator
Table 618.
PROC0_CTRL Register
Bits
Description
Type
Reset
31:2
Reserved.
-
-
1
RUNNING: Is the tick generator running?
RO
-
0
ENABLE: start / stop tick generation
RW
0x0
TICKS: PROC0_CYCLES Register
Offset: 0x04
RP2350 Datasheet
8.5. Tick generators
571


Table 619.
PROC0_CYCLES
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8:0
Total number of clk_tick cycles before the next tick.
RW
0x000
TICKS: PROC0_COUNT Register
Offset: 0x08
Table 620.
PROC0_COUNT
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8:0
Count down timer: the remaining number clk_tick cycles before the next tick is
generated.
RO
-
TICKS: PROC1_CTRL Register
Offset: 0x0c
Description
Controls the tick generator
Table 621.
PROC1_CTRL Register
Bits
Description
Type
Reset
31:2
Reserved.
-
-
1
RUNNING: Is the tick generator running?
RO
-
0
ENABLE: start / stop tick generation
RW
0x0
TICKS: PROC1_CYCLES Register
Offset: 0x10
Table 622.
PROC1_CYCLES
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8:0
Total number of clk_tick cycles before the next tick.
RW
0x000
TICKS: PROC1_COUNT Register
Offset: 0x14
Table 623.
PROC1_COUNT
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8:0
Count down timer: the remaining number clk_tick cycles before the next tick is
generated.
RO
-
TICKS: TIMER0_CTRL Register
Offset: 0x18
Description
Controls the tick generator
Table 624.
TIMER0_CTRL Register
Bits
Description
Type
Reset
31:2
Reserved.
-
-
RP2350 Datasheet
8.5. Tick generators
572


Bits
Description
Type
Reset
1
RUNNING: Is the tick generator running?
RO
-
0
ENABLE: start / stop tick generation
RW
0x0
TICKS: TIMER0_CYCLES Register
Offset: 0x1c
Table 625.
TIMER0_CYCLES
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8:0
Total number of clk_tick cycles before the next tick.
RW
0x000
TICKS: TIMER0_COUNT Register
Offset: 0x20
Table 626.
TIMER0_COUNT
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8:0
Count down timer: the remaining number clk_tick cycles before the next tick is
generated.
RO
-
TICKS: TIMER1_CTRL Register
Offset: 0x24
Description
Controls the tick generator
Table 627.
TIMER1_CTRL Register
Bits
Description
Type
Reset
31:2
Reserved.
-
-
1
RUNNING: Is the tick generator running?
RO
-
0
ENABLE: start / stop tick generation
RW
0x0
TICKS: TIMER1_CYCLES Register
Offset: 0x28
Table 628.
TIMER1_CYCLES
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8:0
Total number of clk_tick cycles before the next tick.
RW
0x000
TICKS: TIMER1_COUNT Register
Offset: 0x2c
RP2350 Datasheet
8.5. Tick generators
573


Table 629.
TIMER1_COUNT
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8:0
Count down timer: the remaining number clk_tick cycles before the next tick is
generated.
RO
-
TICKS: WATCHDOG_CTRL Register
Offset: 0x30
Description
Controls the tick generator
Table 630.
WATCHDOG_CTRL
Register
Bits
Description
Type
Reset
31:2
Reserved.
-
-
1
RUNNING: Is the tick generator running?
RO
-
0
ENABLE: start / stop tick generation
RW
0x0
TICKS: WATCHDOG_CYCLES Register
Offset: 0x34
Table 631.
WATCHDOG_CYCLES
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8:0
Total number of clk_tick cycles before the next tick.
RW
0x000
TICKS: WATCHDOG_COUNT Register
Offset: 0x38
Table 632.
WATCHDOG_COUNT
Register
Bits
Description
Type
Reset
31:9
Reserved.
-
-
8:0
Count down timer: the remaining number clk_tick cycles before the next tick is
generated.
RO
-
TICKS: RISCV_CTRL Register
Offset: 0x3c
Description
Controls the tick generator
Table 633.
RISCV_CTRL Register
Bits
Description
Type
Reset
31:2
Reserved.
-
-
1
RUNNING: Is the tick generator running?
RO
-
0
ENABLE: start / stop tick generation
RW
0x0
TICKS: RISCV_CYCLES Register
Offset: 0x40
RP2350 Datasheet
8.5. Tick generators
574

