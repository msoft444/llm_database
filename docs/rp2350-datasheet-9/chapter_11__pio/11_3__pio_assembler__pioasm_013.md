---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.3. PIO assembler (pioasm)
pages: 886-886
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 11.3. PIO assembler (pioasm)

11.3. PIO assembler (pioasm)

The PIO Assembler parses a PIO source file and outputs the assembled version ready for inclusion in an RP2350

application. This includes C and C++ applications built against the SDK, and Python programs running on the RP2350

MicroPython port.

This section briefly introduces the directives and instructions that can be used in pioasm input. For a deeper discussion

of how to use pioasm, how it is integrated into the SDK build system, extended features such as code pass through, and

the various output formats it can produce, see Raspberry Pi Pico-series C/C++ SDK.
