---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.6. Access control
pages: 823-823
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 10.6. Access control

10.6. Access control

The access control registers (ACCESSCTRL) define permissions required to access GPIOs and bus endpoints such as

peripherals and memory devices.

For each bus endpoint (for example, PIO0), a bus access control register such as PIO0 controls which AHB5 managers

can access it, and at which bus security levels. This register has further implications, such as access to the RESETS

controls for that block. For a full explanation of the bus access control registers, see Section 10.6.2.

For each GPIO, including the QSPI and USB DP/DM pins, a bit in the GPIO_NSMASK0 and GPIO_NSMASK1 register can

be set to make that GPIO accessible to both the Secure and Non-secure domains, or clear to make it Secure-only. This

has system-wide implications, controlling:

• GPIO visibility to the Non-secure SIO

10.5. Secure boot enable procedure
822
