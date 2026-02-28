---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.11.2. Output shift register
pages: 1204-1204
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 12.11.2. Output shift register

![Page 1204 figure](images/fig_p1204.png)

RP2350 Datasheet

is a maximum data rate of 300 Mb/s per pin. There are no limits on the frequency ratio of the system and HSTX clocks,

however each clock must be individually fast enough to maintain your required throughput. Very low system clock

frequencies coupled with very high HSTX frequencies might encounter system DMA bandwidth limitations, since the

DMA is capped at one HSTX FIFO write per system clock cycle.

12.11.1. Data FIFO

An 8-entry, 32-bit-wide FIFO buffers data between the system clock domain (clk_sys) and the HSTX clock domain

(clk_hstx). This is accessed through the AHB FASTPERI arbiter, providing single-cycle write access from the DMA. The

FIFO status is also available through this same bus interface, for faster polled processor IO; see Section 12.11.8.

The FIFO is accessed through a bus interface separate from the control registers (Section 12.11.7), which take multiple

cycles to access due to the asynchronous bus crossing. This design avoids incurring bus stalls on the system DMA or

the FASTPERI arbiter when accessing the FIFO.

The HSTX side also pops 32 bits at a time from the FIFO. The word data stream from the FIFO is optionally manipulated

by the command expander (Section 12.11.5) before being passed to the output shift register.

12.11.2. Output shift register

Figure 127. Every

N_SHIFTS reached?

cycle, the output shift

register either refills

32 bits from the FIFO

or recirculates data

Output Shift 

1

System
Bit Crossbar
Data FIFO
(8 x 32b async)

Register 
(32 bits)

through a right-rotate

/32

function. The rotate

0

can be used to

perform left or right

shifts, and to repeat

Right-rotate
SHIFT = 0-31

data.

The HSTX’s internal data paths are 32 bits wide, but the output is narrower: no more than 16 bits can be output per

HSTX cycle (8 GPIOs × DDR). The output shift register adapts these mismatched data widths. The output shift register

is a 32-bit shift register, which always refills 32 bits at a time, either from the command expander output or directly from

the data FIFO.

The source of data for the output shift register is configured by the CSR.EXPAND_EN field:

• when set, the command expander interposes the FIFO and the output shift register
• when clear, the command expander is bypassed, popping the FIFO directly into the shift register

Whenever CSR.EN is low, the shift register is flushed to empty. Once HSTX has been configured, and EN is set high, the

shift register is ready to accept data, and will pop data as soon as it becomes available.

After popping the first data word, the shift register will now shift every HSTX clock cycle until it becomes empty. The

shift behaviour is configured by:

• CSR.N_SHIFTS, which determines how many times to shift before the register is considered empty
• CSR.SHIFT, which is a right-rotate applied to the shift register every cycle

CSR.N_SHIFTS and CSR.SHIFT must only be changed when CSR.EN is low. It is safe to change these fields in the same

register write that sets EN from low to high.

SHIFT × N_SHIFTS is not necessarily less than or equal to 32. For example, a SHIFT of 31 might be used to shift the register

left by one bit per cycle, since right-rotate is a modular operation, and -1 is equal to 31 under a modulus of 32.

When the shift register is about to become empty, it will immediately refill with fresh data from the command expander

or FIFO if data is available. When data is available, the shift register is never empty for any cycle. If data is not available,

12.11. HSTX
1203
