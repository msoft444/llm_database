---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.2.6. Core-local peripherals (SIO)
pages: 34-35
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 2.2.6. Core-local peripherals (SIO)

![Page 34 figure](images/fig_p0034.png)

2.2.6. Core-local peripherals (SIO)

SIO is accessible to processor load/store only. It contains registers which need single-cycle access from both cores

concurrently, such as the GPIO registers. Access is always zero-wait-state.

2.2. Address map
33

RP2350 Datasheet

| Bus Endpoint | Base Address |
| --- | --- |
| SIO_BASE | 0xd0000000 |
| SIO_NONSEC_BASE | 0xd0020000 |

Table 15. Address

map for SIO bus

segment

![Page 35 figure](images/fig_p0035.png)
