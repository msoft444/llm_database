---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.3.9. ROSC counter
pages: 565-565
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 8.3.9. ROSC counter

8.3.9. ROSC counter

The COUNT register provides a method of managing short software delays. To use this method:

1. Write a value to the COUNT register. The register automatically begins to count down to zero at the ROSC frequency.

2. Poll the register until it reaches zero.

This is preferable to using NOPs in software loops because it is independent of the core clock frequency, the compiler,

and the execution time of the compiled code.
