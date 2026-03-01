---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.2.8. Interactions between state machines
pages: 886-886
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 11.2.8. Interactions between state machines

11.2.8. Interactions between state machines

Instruction memory is implemented as a 1-write, 4-read register file, allowing all four state machines to read an

instruction on the same cycle without stalling.

There are three ways to apply the multiple state machines:

• Pointing multiple state machines at the same program
• Pointing multiple state machines at different programs
• Using multiple state machines to run different parts of the same interface, e.g. TX and RX side of a UART, or

clock/hsync and pixel data on a DPI display

State machines cannot communicate data, but they can synchronise with one another by using the IRQ flags. There are

8 flags total. Each state machine can set or clear any flag using the IRQ instruction, and can wait for a flag to go high or

low using the WAIT IRQ instruction. This allows cycle-accurate synchronisation between state machines.

11.3. PIO assembler (pioasm)

The PIO Assembler parses a PIO source file and outputs the assembled version ready for inclusion in an RP2350

application. This includes C and C++ applications built against the SDK, and Python programs running on the RP2350

MicroPython port.

This section briefly introduces the directives and instructions that can be used in pioasm input. For a deeper discussion

of how to use pioasm, how it is integrated into the SDK build system, extended features such as code pass through, and

the various output formats it can produce, see Raspberry Pi Pico-series C/C++ SDK.
