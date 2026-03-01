---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.1. Overview
pages: 514-514
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 8.1. Overview

![Page 514 figure](images/fig_p0514.png)

RP2350 Datasheet

Chapter 8. Clocks

8.1. Overview

The clocks block provides independent clocks to on-chip and external components. It takes inputs from a variety of

clock sources, allowing the user to trade off performance against cost, board area and power consumption. From these

sources it uses multiple clock generators to provide the required clocks. This architecture allows the user flexibility to

start and stop clocks independently and to vary some clock frequencies whilst maintaining others at their optimum

frequencies.

Figure 33. Clocks

overview

÷

en

GPIO Muxing

clk_gpout0-3

External 
clocks or 
Relaxation 
oscillators

GPCLK0 - 1 

from 
GPIO Muxing

÷
en

ADC

clk_adc

÷
en

USB

clk_usb

USB PLL

÷
en

HSTX

clk_hstx

System PLL

Clock 
sources

÷
en

UART+SPI

clk_peri

Processors, Bus fabric, 

Crystal Oscillator 

÷
en

Memories & 
Memory -mapped registers

clk_sys

(XOSC)

÷

Watchdog & Timers

clk_ref

Ring Oscillator 

(ROSC)

Frequency counter

OTP

Resus

Clocks

switched-core power domain

always-on power domain

Low Power 

Oscillator 

tick
÷
AON Timer

(LPOSC)

clk_pow
Power Manager

External 

clocks

The crystal oscillator (XOSC) provides a reference to two PLLs, which provide high precision clocks to the processors

and peripherals. These are slow to start when waking from the various low-power modes, so the on-chip ring oscillator

(ROSC) is provided to boot the device until they are available. When the switched-core is powered down or the device is

in DORMANT mode (see Section 6.5.3, “DORMANT state”) the on-chip 32kHz low-power oscillator (LPOSC) provides a

clock to the power manager and a tick to the Always-on Timer (AON Timer).

8.1. Overview
513
