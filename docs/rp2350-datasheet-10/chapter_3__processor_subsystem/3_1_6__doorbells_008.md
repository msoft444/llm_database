---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.1.6. Doorbells
pages: 43-44
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 3.1.6. Doorbells

3.1.6. Doorbells

The doorbell registers raise an interrupt on the opposite core. There are 8 doorbell flags in each direction, combined into

a single doorbell interrupt per core. This is a core-local interrupt: the same interrupt number on each core (SIO_IRQ_BELL,

interrupt number 26) notifies that core of incoming doorbell interrupts.

3.1. SIO
42

RP2350 Datasheet

Whereas the mailbox FIFOs are used for cross-core events whose count and order is important, doorbells are used for

events which are accumulative (i.e. may post multiple times, but only answered once) and which can be responded to in

any order.

Writing a non-zero value to the DOORBELL_OUT_SET register raises the opposite core’s doorbell interrupt. The interrupt

remains raised until all bits are cleared. Generally, the opposite core enters its doorbell interrupt handler, reads its

DOORBELL_IN_CLR register to get the mask of active doorbell flags, and then writes back to acknowledge and clear the

interrupt.

The DOORBELL_IN_SET register allows a processor to ring its own doorbell. This is useful when the routine which rings

a doorbell can be scheduled on either core. Likewise, for symmetry, a processor can clear the opposite core’s doorbell

flags using the DOORBELL_OUT_CLR register: this is useful for setup code, but should be avoided in general because of

the potential for race conditions when acknowledging interrupts meant for the opposite core.

At any time, a core can read back its DOORBELL_OUT_SET or DOORBELL_OUT_CLR register (they return the same

result) to see the status of doorbell interrupts posted to the opposite core. Likewise, reading either DOORBELL_IN_SET

or DOORBELL_IN_CLR returns the status of doorbell interrupts posted to this core.

NOTE

RP2350 has separate per-core doorbell interrupt signals and doorbell registers for Secure and Non-secure SIO

banks. Non-secure doorbells are posted on SIO_IRQ_BELL_NS, interrupt number 28. See Section 3.1.1.
