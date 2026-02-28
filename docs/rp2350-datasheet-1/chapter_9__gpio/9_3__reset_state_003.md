---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.3. Reset state
pages: 589-589
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 9.3. Reset state

• Software control via SIO — Section 3.1.3, “GPIO control”
• Flash execute in place (Section 4.4, “External flash and PSRAM (XIP)”) via QSPI Memory Interface (QMI) — Section
12.14, “QSPI memory interface (QMI)”
• UART — Section 12.1, “UART”
• I2C (two-wire serial interface) — Section 12.2, “I2C”
The logical structure of an example IO is shown in Figure 41.
Figure 41. Logical
structure of a GPIO.
Each GPIO can be
controlled by one of a
number of peripherals,
or by software control
registers in the SIO.
The function select
(FSEL) selects which
peripheral output is in
control of the GPIO’s
direction and output
level, and which
peripheral input can
see this GPIO’s input
level. These three
signals (output level,
output enable, input
level) can also be
inverted or forced high
or low, using the GPIO
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
9.3. Reset state
At first power up, Bank 0 IOs (GPIOs 0 through 29 in the QFN-60 package, and GPIOs 0 through 47 in the QFN-80
package) assume the following state:
• Output buffer is high-impedance
• Input buffer is disabled
• Pulled low
• Isolation latches are set to latched (Section 9.7)
The pad output disable bit (GPIO0.OD) for each pad is clear at reset, but the IO muxing is reset to the null function,
RP2350 Datasheet
9.2. Changes from RP2040
588

