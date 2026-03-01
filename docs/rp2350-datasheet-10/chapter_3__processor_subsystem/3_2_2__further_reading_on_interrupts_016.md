---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.2.2. Further reading on interrupts
pages: 84-85
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 3.2.2. Further reading on interrupts

3.2.2. Further reading on interrupts

This section describes the routing of system-level interrupt requests to the processor subsystem. It omits important

details such as the processor’s response to receiving an interrupt, and how processors choose which system-level

interrupt requests to subscribe to. The following is a selection of relevant information for these topics:

• Section 3.7.2.5 describes the Cortex-M33’s internal interrupt controller, the NVIC
• Register listings starting from NVIC_ISER0 describe controls for NVIC operation
• Section 3.7.4.6 is an overview of Cortex-M33 exception handling

3.2. Interrupts
83

RP2350 Datasheet

• The Armv8-M Architecture Reference Manual describes detailed architecture rules for exception handling
• Section 3.8.4 describes standard RISC-V trap handling
• Section 3.8.4.2 describes the standard RISC-V external, timer and software interrupt requests, and how they are

connected on RP2350
• Section 3.8.6.1 describes the Xh3irq interrupt controller, which provides priority-controlled interrupt support for the

system-level interrupts on Hazard3
• Each peripheral has its own interrupt registers which control the assertion of its system-level interrupts listed in

Table 95 — see peripheral documentation for more information
