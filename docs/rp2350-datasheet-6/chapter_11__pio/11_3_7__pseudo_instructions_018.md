---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.3.7. Pseudo-instructions
pages: 890-890
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 11.3.7. Pseudo-instructions

![Page 890 figure](images/fig_p0890.png)

pioasm provides aliases for certain instructions, as a convenience:

nop
Assembles to mov y, y. No side effect, but a useful vehicle for a side-set operation or an extra delay.
