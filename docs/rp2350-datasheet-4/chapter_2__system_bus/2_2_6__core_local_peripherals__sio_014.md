---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.2.6. Core-local peripherals (SIO)
pages: 34-34
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 2.2.6. Core-local peripherals (SIO)

![Page 34 figure](images/fig_p0034.png)

2.2.6. Core-local peripherals (SIO)

SIO is accessible to processor load/store only. It contains registers which need single-cycle access from both cores

concurrently, such as the GPIO registers. Access is always zero-wait-state.

2.2. Address map
33
