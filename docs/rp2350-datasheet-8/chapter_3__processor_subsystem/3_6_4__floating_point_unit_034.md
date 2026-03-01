---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.6.4. Floating point unit
pages: 124-124
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 3.6.4. Floating point unit

3.6.4. Floating point unit

The Cortex-M33 cores on RP2350 are configured with the standard Arm single-precision floating point unit (FPU).

Coprocessor ports 10 and 11 access the FPU.

The Arm floating point extension is documented in the Armv8-M Architecture Reference Manual.

Applications built with the SDK use the FPU automatically by default. For example, calculations with the float data type

in C automatically use the standard FPU, while calculations with the double data type automatically use the RP2350

double-precision coprocessor (Section 3.6.2).
