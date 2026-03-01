---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.2.2. Further reading on interrupts
pages: 84-84
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 3.2.2. Further reading on interrupts

![Page 84 figure](images/fig_p0084.png)

3.2.2. Further reading on interrupts

This section describes the routing of system-level interrupt requests to the processor subsystem. It omits important

details such as the processor’s response to receiving an interrupt, and how processors choose which system-level

interrupt requests to subscribe to. The following is a selection of relevant information for these topics:

• Section 3.7.2.5 describes the Cortex-M33’s internal interrupt controller, the NVIC
• Register listings starting from NVIC_ISER0 describe controls for NVIC operation
• Section 3.7.4.6 is an overview of Cortex-M33 exception handling

3.2. Interrupts
83
