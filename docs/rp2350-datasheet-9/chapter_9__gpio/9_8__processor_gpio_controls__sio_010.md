---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.8. Processor GPIO controls (SIO)
pages: 598-598
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 9.8. Processor GPIO controls (SIO)

9.8. Processor GPIO controls (SIO)

The single-cycle IO subsystem (Section 3.1) contains memory-mapped GPIO registers. The processors can use these to

perform input/output operations on GPIOs:

• The GPIO_OUT and GPIO_HI_OUT registers set the output level: 1 = high, 0 = low
• The GPIO_OE and GPIO_HI_OE registers set the output enable: 1 = output, 0 = input
• The GPIO_IN and GPIO_HI_IN registers read the GPIO inputs

These registers are all 32 bits in size. The low registers (e.g. GPIO_OUT) connect to GPIOs 0 through 31, and the high

registers (e.g. GPIO_HI_OUT) connect to GPIOs 32 through 47, the QSPI pads, and the USB DM/DP pads.

For the output and output enable registers to take effect, the SIO function must be selected on each GPIO (function 5).

However, the GPIO input registers read back the GPIO input values even when the SIO function is not selected, so the

processor can always check the input state of any pin.

The SIO GPIO registers are shared between the two processors and between the Secure and Non-secure security

domains. This avoids programming errors introduced by selecting multiple GPIO functions for access from different

contexts.

Non-secure code’s view of the SIO registers is restricted by the Non-secure GPIO mask defined in GPIO_NSMASK0 and

GPIO_NSMASK1. Non-secure writes to Secure GPIOs are ignored. Non-secure reads of Secure GPIOs return 0.

These registers are documented in more detail in the SIO GPIO register section (Section 3.1.3).

The DMA cannot access registers in the SIO subsystem. The recommended method to DMA to GPIOs is a PIO program

that continuously transfers TX FIFO data to the GPIO outputs, which provides more consistent timing than DMA directly

into GPIO registers.
