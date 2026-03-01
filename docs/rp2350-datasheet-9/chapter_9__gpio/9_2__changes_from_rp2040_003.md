---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.2. Changes from RP2040
pages: 589-589
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 9.2. Changes from RP2040

control registers.

9.2. Changes from RP2040

RP2350 GPIO differs from RP2040 in the following ways:

• 18 more GPIOs in the QFN-80 package
• Addition of a third PIO to GPIO functions
• USB DP/DM pins can be used as GPIO
• Addition of isolation register to pad registers (preserves pad state while in a low power state, cleared by software

on power up)
• Changed default reset state of pad controls
• Both Secure and Non-secure access to GPIOs (see Section 10.6)
• Double the number of GPIO interrupts to differentiate between Secure and Non-secure
• Interrupt summary registers added so you can quickly see which GPIOs have pending interrupts
