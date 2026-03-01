---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.1. Overview
pages: 514-514
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 8.1. Overview

8.1. Overview

The clocks block provides independent clocks to on-chip and external components. It takes inputs from a variety of

clock sources, allowing the user to trade off performance against cost, board area and power consumption. From these

sources it uses multiple clock generators to provide the required clocks. This architecture allows the user flexibility to

start and stop clocks independently and to vary some clock frequencies whilst maintaining others at their optimum

frequencies.

![Page 514 figure](images/fig_p0514.png)

Figure 33. Clocks

overview

External 
clocks or 
Relaxation 
oscillators

The crystal oscillator (XOSC) provides a reference to two PLLs, which provide high precision clocks to the processors

and peripherals. These are slow to start when waking from the various low-power modes, so the on-chip ring oscillator

(ROSC) is provided to boot the device until they are available. When the switched-core is powered down or the device is

in DORMANT mode (see Section 6.5.3, “DORMANT state”) the on-chip 32kHz low-power oscillator (LPOSC) provides a

clock to the power manager and a tick to the Always-on Timer (AON Timer).

8.1. Overview
513
