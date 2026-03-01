---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.9. GPIO coprocessor port
pages: 598-599
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 9.9. GPIO coprocessor port

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

RP2350 Datasheet

9.10. Software examples
