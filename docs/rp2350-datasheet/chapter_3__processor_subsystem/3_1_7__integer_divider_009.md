---
source_pdf: rp2350-datasheet.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.1.7. Integer divider
pages: 44-44
type: technical_spec
generated_at: 2026-03-02T14:11:45.471392+00:00
---

# 3.1.7. Integer divider

RP2040’s memory-mapped integer divider peripheral is not present on RP2350, since the processors support divide

instructions. The address space previously allocated for the divider registers is now reserved.
