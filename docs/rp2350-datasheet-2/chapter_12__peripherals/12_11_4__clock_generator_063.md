---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.11.4. Clock generator
pages: 1206-1206
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 12.11.4. Clock generator

RP2350 Datasheet

◦Same as the previous, but repeats the 8-cycle pattern four times before refreshing the shift register.

◦Rotating by 32 restores the original value that was popped into the shift register from the FIFO or command

expander.

12.11.3.2. Examples: multiple pins

The separation of shift register and bit crossbar allows both zipped and unzipped multi-bit records, once multiple pins

are involved. For example, compare these two configurations:

• N_SHIFTS = 8, SHIFT = 4, BIT0.SEL_P = 0, BIT0.SEL_N = 2, BIT1.SEL_P = 1, BIT1.SEL_N = 3:

◦Each 32-bit word consists of 16 bit-pairs, and a new bit-pair is presented to BIT0 and BIT1 twice per cycle.

◦The shift register advances by 4 every cycle, introducing two new bit-pairs to the rightmost four bits of the

shift register
• N_SHIFTS = 8, SHIFT = 2, BIT0.SEL_P = 0, BIT0.SEL_N = 1, BIT1.SEL_P = 16, BIT1.SEL_N = 17:

◦Each 32-bit word consists of a pair of 16-bit values, each of which is shifted to one pin out of BIT0 and BIT1 at

a rate of two bits per cycle.

◦The shift register advances by two every cycle, introducing a new bit-pair to bits 1:0 for the BIT0 pin, and also

introducing a new bit-pair to bits 17:16 for the BIT1 pin.

Depending on software needs, it might be preferable to pack together all of the bits output on the same cycle (zipped

records), or all of the bits that go through the same pin (unzipped records), so HSTX supports both.

As a final, concrete example, take TMDS (used in DVI): here each 32-bit word contains 3 × 10-bit TMDS symbols, each of

which is serialised to a differential pair over the course of 10 TMDS bit times. For performance, it’s preferable to make

each HSTX clock period equal to two TMDS bit periods, by leveraging the DDR capability. A possible configuration would

therefore be:

• CSR: N_SHIFTS = 5, SHIFT = 2
• BIT0: SEL_P = 0, SEL_N = 1, INV = 0
• BIT1: SEL_P = 0, SEL_N = 1, INV = 1
• BIT2: SEL_P = 10, SEL_N = 11, INV = 0
• BIT3: SEL_P = 10, SEL_N = 11, INV = 1
• BIT4: SEL_P = 20, SEL_N = 21, INV = 0
• BIT5: SEL_P = 20, SEL_N = 21, INV = 1

The missing piece for TMDS is the clock, which has a period of 10 TMDS bit periods, or 5 HSTX clock periods when

shifting two bits per cycle per pin. HSTX has a special-purpose clock generator so that pseudo-clock bits do not have to

be packed into the FIFO data stream. The clock generator is covered in the next section.

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
