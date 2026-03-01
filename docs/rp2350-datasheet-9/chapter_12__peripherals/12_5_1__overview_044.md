---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.5.1. Overview
pages: 1078-1078
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 12.5.1. Overview

RP2350 Datasheet

12.5.1. Overview

Pulse width modulation (PWM) smoothly varies the average voltage of a digital signal using controlled-width positive

pulses at regular intervals. The fraction of time spent high is known as the duty cycle. This may be used to approximate

an analogue output or control switchmode power electronics.

The RP2350 PWM block has 12 identical slices. Each slice can drive two PWM output signals, or measure the frequency

or duty cycle of an input signal. The two outputs on each slice have the same period, but independently varying duty

cycles, so this gives a total of 24 controllable PWM outputs in the QFN-80 package.

![Page 1078 figure](images/fig_p1078.png)

*Figure 111. A single PWM slice. A 16-bit counter counts from 0 up to some programmed value, and then wraps to zero, or counts back down again, depending on PWM mode. The A and B outputs transition high*

and low based on the

current count value

and the

preprogrammed A and

B thresholds. The

Each PWM slice is equipped with the following:

counter advances

based on a number of

• 16-bit counter
• 8.4 fractional clock divider
• Two independent output channels, duty cycle from 0% to 100% inclusive
• Dual slope and trailing edge modulation
• Edge-sensitive input mode for frequency measurement
• Level-sensitive input mode for duty cycle measurement
• Configurable counter wrap value

events: it may be free-

running, or gated by

level or edge of an

input signal on the B

pin. A fractional

divider slows the

overall count rate for

finer control of output

frequency.

◦Wrap and level registers are double buffered and can be changed race-free while PWM is running
• Interrupt request and DMA request on counter wrap
• Phase can be precisely advanced or retarded while running (increments of one count)

Slices can be enabled or disabled simultaneously via a single global control register. Slices then run in lockstep, so that

more complex power circuitry can be switched by the outputs of multiple slices.

12.5.1.1. Changes from RP2040

• Increased the number of slices from 8 to 12, with the 4 additional slices available on GPIOs 32 through 47 in the

QFN-80 package.
• Added a second shared interrupt line (controlled by IRQ1_INTE), to aid use of PWM slices as simple repeating

timers.
