---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.6.2. Changes from RP2040
pages: 576-576
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 8.6.2. Changes from RP2040

Table 634.
RISCV_CYCLES
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
TICKS: RISCV_COUNT Register
Offset: 0x44
Table 635.
RISCV_COUNT
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
8.6. PLL
8.6.1. Overview
The PLL takes a reference clock and multiplies it using a Voltage Controlled Oscillator (VCO) with a feedback loop. The
VCO runs at high frequencies: between 750 MHz and 1600 MHz. As a result, there are two post dividers that can divide
the VCO frequency before it is distributed to the clock generators on the chip.
There are two PLLs in RP2350. They are:
• pll_sys - used to generate up to a 150 MHz system clock
• pll_usb - used to generate a 48 MHz USB reference clock
FREF
REFDIV
FBDIV
LOCK
FOUTVCO
CLKSSCG
Analog circuits
Post divider rate circuits
Reference rate circuits
FOUTPOSTDIV
Lock Detect
Feedback Divide
÷16-320
BYPASS
POSTDIV1
POSTDIV2
÷1-7
÷1-7
PFD
÷1-63
6'b
3'b
3'b
12'b
VCO
Figure 40. On both
PLLs, the FREF
(reference) input is
connected to the
crystal oscillator’s XIN
(XI) input. The PLL
contains a VCO, which
is locked to a constant
ratio of the reference
clock via the feedback
loop (phase-frequency
detector and loop
filter). This can
synthesise very high
frequencies, which
may be divided down
by the post-dividers.
The routing between PLLs and system-level clocks is flexible. For example, you could run USB off a division of the
system PLL (e.g. 144 MHz / 3 = 48 MHz), leaving the USB PLL free for other uses such as the HSTX peripheral or a
general-purpose clock output on a GPIO.
8.6.2. Changes from RP2040
• RP2350 added an interrupt that fires if the PLL loses lock. See CS.LOCK_N.
RP2350 Datasheet
8.6. PLL
575

