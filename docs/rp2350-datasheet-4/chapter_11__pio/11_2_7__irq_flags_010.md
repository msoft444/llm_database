---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.2.7. IRQ flags
pages: 885-885
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 11.2.7. IRQ flags

11.2.7. IRQ flags

IRQ flags are state bits which can be set or cleared by state machines or the system. There are 8 in total: all 8 are visible

to all state machines, and the lower 4 can also be masked into one of PIO’s interrupt request lines, via the IRQ0_INTE and

IRQ1_INTE control registers.

They have two main uses:

11.2. Programmer’s model
884
