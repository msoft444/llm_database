---
source_pdf: rp2350-datasheet.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.6. Cortex-M33 coprocessors
pages: 101-102
type: technical_spec
generated_at: 2026-03-02T14:11:45.471392+00:00
---

# 3.6. Cortex-M33 coprocessors

The Cortex-M33 features a coprocessor port which transfers up to 64 bits per cycle between the processor and certain

closely-coupled hardware. The Cortex-M33’s built-in floating-point unit is an example of such a coprocessor, but

RP2350 adds three device-specific coprocessors to this interface. The following sections document these

coprocessors.

Before accessing a coprocessor from Secure code, that coprocessor must first be enabled by setting the corresponding

bit in the CPACR. Before accessing from the Non-secure state, the corresponding bits in the NSACR and CPACR_NS

registers must be set.

The RISC-V processors on RP2350 do not have access to the Cortex-M33 coprocessors.
