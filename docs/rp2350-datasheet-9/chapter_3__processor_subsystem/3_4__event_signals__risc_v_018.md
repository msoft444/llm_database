---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.4. Event signals (RISC-V)
pages: 85-85
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 3.4. Event signals (RISC-V)

3.4. Event signals (RISC-V)

The Hazard3 h3.block instruction halts processor execution until an unblock signal is received. The h3.unblock instruction

sends an unblock signal to other processors. These NOP-compatible hint instructions are documented in Section

3.8.6.3.

On RP2350 the Hazard3 unblock in/out signals are cross-connected between the two processors, and each processor’s

unblock output is also fed back into its input. The global monitor also posts an unblock signal to each core when that

core loses a reservation due to an access by another core or the system DMA.

The Hazard3 MSLEEP CSR defines how deep a sleep the processor will enter when executing a h3.block instruction. By

default this is a simple pipeline stall, but the processor can also gate its own clock and negotiate the system-level clock

wake/sleep state with the clocks block (Section 6.5.2).

The h3.unblock instruction is "sticky": an h3.block will fall through immediately if any unblock signal has been received

since the last time the processor executed an h3.block instruction.
