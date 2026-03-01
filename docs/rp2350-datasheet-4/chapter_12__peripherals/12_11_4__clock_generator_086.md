---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.11.4. Clock generator
pages: 1206-1206
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.11.4. Clock generator

12.11.4. Clock generator

The clock generator is a counter that provides a periodic signal over the course of n HSTX clock cycles, configured by

CSR.CLKDIV. The clock period is always an integer number of HSTX clock cycles, in the range 1 to 16 inclusive. The

clock generator supports both odd and even periods, using the DDR outputs to support mid-HSTX-cycle output

transitions. There is only a single clock generator — to emulate multiple clocks, pack pseudo-clock bits into FIFO data.

The clock generator increments on cycles where the output shift register is shifted. Generally, the clock period will be a

divisor of CSR.N_SHIFTS so that clock and data maintain a consistent alignment. In the TMDS example in the previous

section, a CLKDIV of 5 would be suitable, so that the clock repeats every time the shift register refreshes. This matches

the requirement for a TMDS clock period of 10 bit periods, since two bits are transferred every cycle.

The clock generator output is connected to any pin whose BITx.CLK bit is set (e.g. BIT0.CLK). To produce differential

12.11. HSTX
1205
