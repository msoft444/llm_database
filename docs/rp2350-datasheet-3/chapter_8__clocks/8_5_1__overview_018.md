---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.5.1. Overview
pages: 571-571
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 8.5.1. Overview

RP2350 Datasheet

Frequency drift with power supply voltage: ±20%.

8.4.2. Using an external low-power clock

Instead of using the low-power RC oscillator, an external 32.768 kHz low power clock signal can be provided on one of

GPIO 12, 14, 20, or 22. Alternatively, those GPIOs can be used to provide a 1 kHz or 1 Hz tick. See Section 12.10.5.2,

“Using an external clock in place of LPOSC” and Section 12.10.7, “Using an external clock or tick from GPIO” for more

details.

8.4.3. List of registers

The low power oscillator shares register address space with other power management subsystems in the always-on

domain. The address space is referred to as POWMAN elsewhere in this document. A complete list of POWMAN

registers is provided in Section 6.4, “Power management (POWMAN) registers”, but information on registers associated

with the low power oscillator is repeated here.

The POWMAN registers start at a base address of 0x40100000 (defined as POWMAN_BASE in SDK).

• LPOSC
• EXT_TIME_REF
• LPOSC_FREQ_KHZ_INT
• LPOSC_FREQ_KHZ_FRAC

8.5. Tick generators

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
