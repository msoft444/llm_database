---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.2.5. XOSC counter
pages: 558-558
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 8.2.5. XOSC counter

8.2.5. XOSC counter

The COUNT register provides a method of managing short software delays. To use this method:

1. Write a value to the COUNT register. The register automatically begins to count down to zero at the XOSC frequency.

2. Poll the register until it reaches zero.

This is preferable to using NOPs in software loops because it is independent of the core clock frequency, the compiler,

and the execution time of the compiled code.
