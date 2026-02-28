---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.1. Overview
pages: 514-514
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 8.1. Overview

Chapter 8. Clocks
8.1. Overview
The clocks block provides independent clocks to on-chip and external components. It takes inputs from a variety of
clock sources, allowing the user to trade off performance against cost, board area and power consumption. From these
sources it uses multiple clock generators to provide the required clocks. This architecture allows the user flexibility to
start and stop clocks independently and to vary some clock frequencies whilst maintaining others at their optimum
frequencies.
GPCLK0 - 1 
from 
GPIO Muxing
External 
clocks or 
Relaxation 
oscillators
External 
clocks
Clock 
sources
clk_gpout0-3
clk_adc
clk_usb
clk_hstx
clk_peri
clk_sys
clk_ref
switched-core power domain
always-on power domain
÷
÷
en
÷
en
÷
en
÷
en
÷
en
USB PLL
System PLL
Crystal Oscillator 
(XOSC)
Ring Oscillator 
(ROSC)
Low Power 
Oscillator 
(LPOSC)
÷
Frequency counter
Resus
Clocks
GPIO Muxing
ADC
USB
HSTX
UART+SPI
Processors, Bus fabric, 
Memories & 
Memory -mapped registers
Watchdog & Timers
OTP
tick
÷
AON Timer
clk_pow
Power Manager
en
Figure 33. Clocks
overview
The crystal oscillator (XOSC) provides a reference to two PLLs, which provide high precision clocks to the processors
and peripherals. These are slow to start when waking from the various low-power modes, so the on-chip ring oscillator
(ROSC) is provided to boot the device until they are available. When the switched-core is powered down or the device is
in DORMANT mode (see Section 6.5.3, “DORMANT state”) the on-chip 32kHz low-power oscillator (LPOSC) provides a
clock to the power manager and a tick to the Always-on Timer (AON Timer).
RP2350 Datasheet
8.1. Overview
513

