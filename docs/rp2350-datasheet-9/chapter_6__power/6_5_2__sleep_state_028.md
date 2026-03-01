---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.5.2. SLEEP state
pages: 490-490
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 6.5.2. SLEEP state

6.5.2. SLEEP state

RP2350 enters the SLEEP state when all of the following are true:

• Both processors are asleep (e.g. in a WFE or WFI instruction)
• The system DMA has no outstanding transfers on any channel

RP2350 exits the SLEEP state when either processor is awoken by an interrupt.

When in the SLEEP state, the top-level clock gates are masked by the SLEEP_ENx registers (starting at SLEEP_EN0), rather

than the WAKE_ENx registers (starting at WAKE_EN0). This permits more aggressive pruning of the clock tree when the

processors are asleep.

NOTE

Though it is possible for a clock to be enabled during SLEEP and disabled outside of SLEEP, this is generally not

useful.

For example, if the system is sleeping until a character interrupt from a UART, the entire system except for the UART

can be clock-gated (SLEEP_ENx = all-zeroes except for CLK_SYS_UART0 and CLK_PERI_UART0). This includes system

infrastructure such as the bus fabric.

When the UART asserts its interrupt and wakes a processor, RP2350 leaves SLEEP mode and switches back to the

WAKE_ENx clock mask. At the minimum, this should include the bus fabric and the memory devices containing the

processor’s stack and interrupt vectors.

A system-level clock request handshake holds the processors off the bus until the clocks are re-enabled.
