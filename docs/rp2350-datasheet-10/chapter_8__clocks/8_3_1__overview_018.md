---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.3.1. Overview
pages: 562-563
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 8.3.1. Overview

8.3.1. Overview

The Ring Oscillator (ROSC) is an on-chip oscillator built from a ring of inverters. It requires no external components and

is started automatically during RP2350 power up. It provides the clock to the cores during boot. The frequency of the

ROSC is programmable and it can directly provide a high speed clock to the cores, but the frequency varies with

Process, Voltage, and Temperature (PVT) so it cannot provide clocks for components that require an accurate

frequency such as the AON Timer, USB, and ADC. The frequency can be randomised to provide some protection against

attempts to recover the system clock from power traces. Methods for mitigating unwanted frequency variation are

discussed in Section 8.1, “Overview”, but these are only relevant to very low power designs. For most applications

requiring accurate clock frequencies, switch to the XOSC and PLLs. During boot, the ROSC runs at a nominal 11MHz

and is guaranteed to be in the range 4.6MHz to 19.6MHz without randomisation and 4.6MHz to 24.0MHz with

randomisation.

NOTE

RP2350 A3 and later enable randomisation by default, and the bootrom quadruples the ROSC base frequency by

reducing DIV to 2. As a result, clk_sys is guaranteed to range between 18.4 MHz and 96.0 MHz. clk_ref is maintained

at a nominal 11 MHz by increasing its divisor. This change increases the sensitivity of the glitch detectors, which

have an inverse relationship with clock period, consequently better protecting the ROM’s early boot paths.

After the chip has booted, the programmer can choose to continue running from the ROSC and increase its frequency or

start the Crystal Oscillator (XOSC) and PLLs. You can disable the ROSC when you’ve switched the system clocks to the

XOSC. Each oscillator has advantages; switch between them to achieve the best solution for your application.

8.3. Ring oscillator (ROSC)
561

RP2350 Datasheet

![Page 563 figure](images/fig_p0563.png)

*Figure 39. ROSC overview.*
