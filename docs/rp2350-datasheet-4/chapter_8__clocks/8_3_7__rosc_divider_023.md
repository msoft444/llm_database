---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.3.7. ROSC divider
pages: 564-564
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 8.3.7. ROSC divider

![Page 564 figure](images/fig_p0564.png)

8.3.7. ROSC divider

The ROSC frequency is too fast to be used directly, so it is divided in an integer divider controlled by the DIV register. You

can change DIV while the ROSC is running, and the output clock will change frequency without glitching. The default

divisor is 8, which ensures the output clock is in the specified range on chip startup.

The divider has two outputs, rosc_clksrc and rosc_clksrc_ph. rosc_clksrc_ph is a phase shifted version of rosc_clksrc. This

is primarily intended for use during product development; the outputs are identical if the PHASE register is left in its

default state.

8.3. Ring oscillator (ROSC)
563
