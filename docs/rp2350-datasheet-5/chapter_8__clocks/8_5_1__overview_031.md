---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.5.1. Overview
pages: 571-572
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 8.5.1. Overview

8.5.1. Overview

The tick generators provide time references for several blocks:

• System timers: TIMER0 and TIMER1 (Section 12.8, “System timers”)
• RISC-V platform timer (Section 3.1.8, “RISC-V platform timer”)
• Arm Cortex-M33 SysTick timers for core 0 and core 1
• The watchdog timer (Section 12.9, “Watchdog”)

A tick is a periodic signal which provides a timebase for a timer or counter. These signals are similar to clocks, although

they do not drive the clock inputs of any registers on the chip. The use of ticks as opposed to clocks makes it simpler to

distribute timebase information that is independent of any subsystem clocks. For example, the system timers (TIMER0

and TIMER1) should continue to count once per microsecond even as the system clock varies according to processor

demand.

The tick generators use clk_ref as their reference clock (see Section 8.1, “Overview” for an overview of system-level

clocks including clk_ref). Ideally, clk_ref will be configured to use the crystal oscillator (Section 8.2, “Crystal oscillator

(XOSC)”) to provide an accurate reference. The generators divide clk_ref internally to generate a tick signal for each

destination.

The SDK expects a nominal 1 μs timebase for the system timers and the RISC-V platform timer. Similarly the Cortex-

M33 SysTick timers require a 1 μs timebase to match the hardwired value of 100,000 in the SYST_CALIB register, which

standard Arm software uses to scale SysTick delays. However, you may need to scale these timebases differently if

your software has specific requirements such as a longer maximum delay on the 24-bit SysTick peripherals. The tick

generator can scale each destination’s tick timebase independently of the others.

For a 12 MHz reference clock, set the cycle count to 12 to generate a 1 μs tick. A 1 MHz clock has a period of 1 μs, so

8.5. Tick generators
570

RP2350 Datasheet

the hardware needs to count for 12 times as many clock cycles to get a 1 μs tick from a reference running at 12 ×

1 MHz.

Before changing the cycle count, always stop the tick generator with the TIMER0_CTRL.ENABLE bit. You can re-enable

once the tick generator is configured.

![Page 572 figure](images/fig_p0572.png)
