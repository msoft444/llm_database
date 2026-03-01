---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.6.1. Overview
pages: 576-576
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 8.6.1. Overview

8.6.1. Overview

The PLL takes a reference clock and multiplies it using a Voltage Controlled Oscillator (VCO) with a feedback loop. The

VCO runs at high frequencies: between 750 MHz and 1600 MHz. As a result, there are two post dividers that can divide

the VCO frequency before it is distributed to the clock generators on the chip.

There are two PLLs in RP2350. They are:

• pll_sys - used to generate up to a 150 MHz system clock
• pll_usb - used to generate a 48 MHz USB reference clock

![Page 576 figure](images/fig_p0576.png)

*Figure 40. On both PLLs, the FREF (reference) input is connected to the crystal oscillator’s XIN (XI) input. The PLL contains a VCO, which is locked to a constant ratio of the reference clock via the feedback loop (phase-frequency detector and loop filter). This can synthesise very high frequencies, which may be divided down by the post-dividers.*

The routing between PLLs and system-level clocks is flexible. For example, you could run USB off a division of the

system PLL (e.g. 144 MHz / 3 = 48 MHz), leaving the USB PLL free for other uses such as the HSTX peripheral or a

general-purpose clock output on a GPIO.
