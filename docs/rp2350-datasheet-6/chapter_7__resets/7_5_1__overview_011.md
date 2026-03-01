---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.5.1. Overview
pages: 504-504
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 7.5.1. Overview

7.5.1. Overview

The reset controller allows software to reset non-critical components in RP2350. The reset controller can reset the

following components:

• USB Controller
• PIO
• Peripherals, including UART, I2C, SPI, PWM, Timer, ADC
• PLLs
• IO and Pad registers

For a full list of components that can be reset using the reset controller, see the register descriptions (Section 7.5.3,

“List of Registers”).

When reset, components are held in reset at power-up. To use the component, software must deassert the reset.

NOTE

The SDK automatically deasserts some components after a reset.
