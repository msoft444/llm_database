---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.9. GPIO coprocessor port
pages: 598-598
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 9.9. GPIO coprocessor port

RP2350 Datasheet

(GPIO0.IE) resets to 0 (input-disabled), so the isolation latches for these signals also take a reset value of 0. Resetting

the isolation latch forces the pad to assume its reset state even if it is currently isolated.

The ISO control bits (e.g. GPIO0.ISO) are reset by the top-level switched core domain isolation signal, which is asserted

by POWMAN before powering down the switched core domain and de-asserted after it is powered up. This means that

entering and exiting a sleep state where the switched core domain is unpowered leaves all GPIOs isolated after power

up; you can then re-engage them individually. The ISO control bits are not reset by the PADS register block reset driven

by the RESETS control registers: resetting the PADS register block returns non-isolated pads to their reset state, but has

no effect on isolated pads.

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

9.9. GPIO coprocessor port

Coprocessor port 0 on each Cortex-M33 processor connects to a GPIO coprocessor interface. These coprocessor

instructions provide fast access to the SIO GPIO registers from Arm software:

• The equivalent of any SIO GPIO register access is a single instruction, without having to materialise a 32-bit

register address beforehand
• An indexed write operation on any single GPIO is a single instruction
• 64 bits can be read/written in a single instruction

This reduces the timing impact of GPIO accesses on surrounding software, for example when GPIO tracing has been

added to interrupt handlers diagnose complex timing issues.

Both Secure and Non-secure code may access the coprocessor. Non-secure code sees a restricted view of the GPIO

registers, defined by ACCESSCTRL GPIO_NSMASK0/1.

The GPIO coprocessor instruction set is documented in Section 3.6.1.

9.8. Processor GPIO controls (SIO)
597
