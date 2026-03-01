---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.11.3. Bit crossbar
pages: 1205-1205
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.11.3. Bit crossbar

RP2350 Datasheet

the shift register becomes empty and stops shifting until more data is provided. Once data is provided, the shift register

refills and begins shifting once again.

12.11.3. Bit crossbar

The bit crossbar controls which bits of the output shift register appear on which GPIOs during the first and second half

of each HSTX clock cycle. There is a configuration register for each pin, BIT0 through BIT7:

• BITx.SEL_P selects which shift register bit (0 through 31) is output for the first half of each HSTX clock cycle
• BITx.SEL_N selects which shift register bit (0 through 31) is output for the second half of each clock cycle
• BITx.INV inverts the output (logical NOT)
• BITx.CLK indicates that this pin should be connected to the clock generator (Section 12.11.4) rather than the output

shift register

To disable DDR behaviour set SEL_N equal to SEL_P. To implement a differential output, configure two pins identically

except for the INV bit, which should be set for one pin and clear for the other.

12.11.3.1. Examples: one pin

Together with the SHIFT and N_SHIFTS controls for the shift register, the pin configuration determines the data layout

passed through the HSTX. Since not all of us are accustomed to thinking in four dimensions, it’s worth going through

some examples with a single pin:

• N_SHIFTS = 32, SHIFT = 1, SEL_P = 0, SEL_N = 0:

◦Shift out one bit per HSTX clock cycle, LSB-first.

◦Each cycle, the shift register advances to the right by one, and the least-significant bit at that time is

presented to the pin for both halves of the cycle, since SEL_P and SEL_N both select the same bit.
• N_SHIFTS = 32, SHIFT = 31, SEL_P = 31, SEL_N = 31:

◦Shift out one bit per HSTX clock cycle, MSB-first.

◦Each cycle, the shift register advances to the left by one (or rather, wraps around the right-hand edge of the

register and ends up one bit left of where it started), and the most-significant bit at that time is presented to

the pin.
• N_SHIFTS = 16, SHIFT = 2, SEL_P = 0, SEL_N = 1:

◦Shift out two bits per HSTX clock cycle, LSB-first.

◦Each cycle, the shift register advances to the right by two. The least-significant bit is presented to the pin for

the first half of that cycle, and the neighbouring bit is presented for the second half.
• N_SHIFTS = 16, SHIFT = 30, SEL_P = 31, SEL_N = 30:

◦Shift out two bits per HSTX clock cycle, MSB-first.

◦Each cycle, the shift register advances to the left by two. The most-significant bit is presented to the pin for

the first half of that cycle, and the neighbouring bit is presented for the latter half.
• N_SHIFTS = 8, SHIFT = 4, SEL_P = 0, SEL_N = 0:

◦Shift out the least-significant bit in each group of 4 bits, over the course of 8 clock cycles.

◦Each cycle, the shift register advances by to the right by four. The least-significant bit of the shift register is

presented to the pin. The bit indices presented to the pin are therefore 0, 4, 8, 12, 16, 20, 24, and 28.
• N_SHIFTS = 32, SHIFT = 4, SEL_P = 0, SEL_N = 0:

12.11. HSTX
1204
