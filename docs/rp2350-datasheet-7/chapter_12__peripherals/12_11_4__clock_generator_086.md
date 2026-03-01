---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.11.4. Clock generator
pages: 1206-1207
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
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

RP2350 Datasheet

clock outputs, connect the clock to two pins, and invert one of them.

The CSR.CLKPHASE field defines the initial phase (count) of the clock generator, configured in units of one half HSTX

clock cycle. The clock generator resets whenever CSR.EN is low and holds at this initial phase. Once CSR.EN is set and

the output shift register begins to shift, the clock generator advances.

Clock generator output whilst CSR.EN is low is determined by the relation of clock period and initial clock phase: if the

initial clock phase is less than one half clock period, then the output is initially low. Otherwise, it is initially high. The

clock generator can be thought of as being low for the first half of each generation period, and high for the second half.

The maximum CSR.CLKPHASE is only 15 half HSTX clock cycles. The maximum CSR.CLKDIV is 16 full HSTX clock

cycles: initial phases of greater than or equal to 180 degrees with the maximum clock period require the inversion of the

clock using the bit crossbar inversion controls.

Only change CSR.CLKPHASE and CSR.CLKDIV when CSR.EN is low. It is safe to modify them in the same register write

that sets EN from low to high.

12.11.4.1. Example: centre-aligned clock

When transmitting source-synchronous data, the data sink (the receiver) must not see data transitions too late before or

too soon after the active edges of the clock. Violating these setup and hold constraints can lead to undefined operation

of the external data sink.

Since the HSTX output delays are all mutually balanced, you can meet these constraints by placing clock transitions

halfway between data transitions, known as centre-aligned clocking.

Since this positions the clock with a temporal resolution of one half of a bit time, the maximum data rate is one bit per

HSTX clock cycle per pin. Because the clock already uses DDR, you cannot use DDR to increase the data rate. Therefore

for all BIT0 through BIT7, BITx.SEL_N is equal to BITx.SEL_P.

For single-data-rate data, with an active rising edge, use the following clock generator settings:

• CSR.CLKDIV = 1 (1 HSTX clock period)
• CSR.CLKPHASE = 1 (1/2 HSTX clock period)

The clock is delayed by half an HSTX cycle, to offset it from the launch of the first data.

For single-data-rate data, with an active falling edge, use the following clock generator settings:

• CSR.CLKDIV = 1 (1 HSTX clock period)
• CSR.CLKPHASE = 2 (1 HSTX clock period)

Alternatively, you could use the same settings as an active-rising edge clock, with the clock output inverted via the bit

crossbar configuration.

For double-data-rate data, with active rising and active falling edges, use the following clock generator settings:

• CSR.CLKDIV = 2 (2 HSTX clock period)
• CSR.CLKPHASE = 1 (1/2 HSTX clock period)

In all three cases, the data rate is the same, at 1 bit per HSTX clock cycle, per pin.
