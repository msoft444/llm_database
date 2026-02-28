---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.5. Interrupts
pages: 595-595
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 9.5. Interrupts

![Page 595 figure](images/fig_p0595.png)

RP2350 Datasheet

| Function Name | Description |
| --- | --- |
| SIO | Software control of GPIO, from the single-cycle IO (SIO) block. The SIO function (F5) must be selected
for the processors to drive a GPIO, but the input is always connected, so software can check the state
of GPIOs at any time. |
| QMI | QSPI memory interface peripheral, used for execute-in-place from external QSPI flash or PSRAM
memory devices. |

The six QSPI Bank GPIO pins are typically used by the XIP peripheral to communicate with an external flash device.

However, there are two scenarios where the pins can be used as software-controlled GPIOs:

• If a SPI or Dual-SPI flash device is used for execute-in-place, then the SD2 and SD3 pins are not used for flash

access, and can be used for other GPIO functions on the circuit board.
• If RP2350 is used in a flashless configuration (USB and OTP boot only), then all six pins can be used for software-

controlled GPIO functions.

9.5. Interrupts

An interrupt can be generated for every GPIO pin in four scenarios:

• Level High: the GPIO pin is a logical 1
• Level Low: the GPIO pin is a logical 0
• Edge High: the GPIO has transitioned from a logical 0 to a logical 1
• Edge Low: the GPIO has transitioned from a logical 1 to a logical 0

The level interrupts are not latched. This means that if the pin is a logical 1 and the level high interrupt is active, it will

become inactive as soon as the pin changes to a logical 0. The edge interrupts are stored in the INTR register and can be

cleared by writing to the INTR register.

There are enable, status, and force registers for three interrupt destinations: proc 0, proc 1, and dormant_wake. For proc

0 the registers are enable (PROC0_INTE0), status (PROC0_INTS0), and force (PROC0_INTF0). Dormant wake is used to

wake the ROSC or XOSC up from dormant mode. See Section 6.5.6.2 for more information on dormant mode.

There is an interrupt output for each combination of IO bank, IRQ destination, and security domain. In total there are

twelve such outputs:

• IO Bank 0 to dormant wake (Secure and Non-secure)
• IO Bank 0 to proc 0 (Secure and Non-secure)
• IO Bank 0 to proc 1 (Secure and Non-secure)
• IO QSPI to dormant wake (Secure and Non-secure)
• IO QSPI to proc 0 (Secure and Non-secure)
• IO QSPI to proc 1 (Secure and Non-secure)

Each interrupt output has its own array of enable registers (INTE) that configures which GPIO events cause the interrupt

to assert. The interrupt asserts when at least one enabled event occurs, and de-asserts when all enabled events have

been acknowledged via the relevant INTR register.

This means the user can watch for several GPIO events at once.

Summary registers can be used to quickly check for pending GPIO interrupts. See IRQSUMMARY_PROC0_NONSECURE0

for an example.

9.5. Interrupts
594
