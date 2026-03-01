---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.1.7. Bus performance counters
pages: 31-31
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 2.1.7. Bus performance counters

2.1.7. Bus performance counters

Bus performance counters automatically count accesses to the main AHB5 crossbar arbiters. These counters can help

diagnose high-traffic performance issues.

There are four performance counters, starting at PERFCTR0. Each is a 24-bit saturating counter. Counter values can be

read from BUSCTRL_PERFCTRx and cleared by writing any value to BUSCTRL_PERFCTRx. Each counter can count one of the 20

available events at a time, as selected by BUSCTRL_PERFSELx. For more information, see Section 12.15.4.
