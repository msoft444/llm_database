---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.2. Programmer’s model
pages: 879-879
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 11.2. Programmer’s model

11.2. Programmer’s model

The four state machines execute from shared instruction memory. System software loads programs into this memory,

configures the state machines and IO mapping, and then sets the state machines running. PIO programs come from

various sources: assembled directly by the user, drawn from the PIO library, or generated programmatically by user

software.

From this point on, state machines are generally autonomous, and system software interacts through DMA, interrupts

and control registers, as with other peripherals on RP2350. For more complex interfaces, PIO provides a small but

flexible set of primitives which allow system software to be more hands-on with state machine control flow.

11.2. Programmer’s model
878
