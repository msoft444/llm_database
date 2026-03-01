---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.1.6. Doorbells
pages: 43-43
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 3.1.6. Doorbells

3.1.6. Doorbells

The doorbell registers raise an interrupt on the opposite core. There are 8 doorbell flags in each direction, combined into

a single doorbell interrupt per core. This is a core-local interrupt: the same interrupt number on each core (SIO_IRQ_BELL,

interrupt number 26) notifies that core of incoming doorbell interrupts.

3.1. SIO
42
