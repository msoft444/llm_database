---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.11.7. List of control registers
pages: 1209-1213
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 12.11.7. List of control registers

12.11.7. List of control registers

The control registers start at a base address of 0x400c0000 (defined as HSTX_CTRL_BASE in the SDK). They are

accessed through an asynchronous bus crossing, so each bus access takes several cycles, the exact figure depending

on the ratio of clk_sys and clk_hstx.

| Offset | Name | Info |
| --- | --- | --- |
| 0x00 | CSR |  |
| 0x04 | BIT0 | Data control register for output bit 0 |
| 0x08 | BIT1 | Data control register for output bit 1 |
| 0x0c | BIT2 | Data control register for output bit 2 |
| 0x10 | BIT3 | Data control register for output bit 3 |
| 0x14 | BIT4 | Data control register for output bit 4 |
| 0x18 | BIT5 | Data control register for output bit 5 |
| 0x1c | BIT6 | Data control register for output bit 6 |
| 0x20 | BIT7 | Data control register for output bit 7 |
| 0x24 | EXPAND_SHIFT | Configure the optional shifter inside the command expander |
| 0x28 | EXPAND_TMDS | Configure the optional TMDS encoder inside the command expander |

Table 1253. List of

12.11. HSTX
1208

RP2350 Datasheet

HSTX_CTRL: CSR Register

Offset: 0x00

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:28 | CLKDIV: Clock period of the generated clock, measured in HSTX clock cycles. Can be odd or even. The generated clock advances only on cycles where the shift register shifts. For example, a clkdiv of 5 would generate a complete output clock period for every 5 HSTX clocks (or every 10 half-clocks). A CLKDIV value of 0 is mapped to a period of 16 HSTX clock cycles. | RW | 0x1 |
| 27:24 | CLKPHASE: Set the initial phase of the generated clock. A CLKPHASE of 0 means the clock is initially low, and the first rising edge occurs after one half period of the generated clock (i.e. CLKDIV/2 cycles of clk_hstx). Incrementing CLKPHASE by 1 will advance the initial clock phase by one half clk_hstx period. For example, if CLKDIV=2 and CLKPHASE=1: * The clock will be initially low * The first rising edge will be 0.5 clk_hstx cycles after asserting first data * The first falling edge will be 1.5 clk_hstx cycles after asserting first data This configuration would be suitable for serialising at a bit rate of clk_hstx with a centre-aligned DDR clock. When the HSTX is halted by clearing CSR_EN, the clock generator will return to its initial phase as configured by the CLKPHASE field. Note CLKPHASE must be strictly less than double the value of CLKDIV (one full period), else its operation is undefined. | RW | 0x0 |
| 23:21 | Reserved. | - | - |
| 20:16 | N_SHIFTS: Number of times to shift the shift register before refilling it from the FIFO. (A count of how many times it has been shifted, not the total shift distance.) A register value of 0 means shift 32 times. | RW | 0x05 |
| 15:13 | Reserved. | - | - |
| 12:8 | SHIFT: How many bits to right-rotate the shift register by each cycle. The use of a rotate rather than a shift allows left shifts to be emulated, by subtracting the left-shift amount from 32. It also allows data to be repeated, when the product of SHIFT and N_SHIFTS is greater than 32. | RW | 0x06 |
| 7 | Reserved. | - | - |
| 6:5 | COUPLED_SEL: Select which PIO to use for coupled mode operation. | RW | 0x0 |
| 4 | COUPLED_MODE: Enable the PIO-to-HSTX 1:1 connection. The HSTX must be clocked directly from the system clock (not just from some other clock source of the same frequency) for this synchronous interface to function correctly. When COUPLED_MODE is set, BITx_SEL_P and SEL_N indices 24 through 31 will select bits from the 8-bit PIO-to-HSTX path, rather than shifter bits. Indices of 0 through 23 will still index the shift register as normal. The PIO outputs connected to the PIO-to-HSTX bus are those same outputs that would appear on the HSTX-capable pins if those pins' FUNCSELs were set to PIO instead of HSTX. For example, if HSTX is on GPIOs 12 through 19, then PIO outputs 12 through 19 are connected to the HSTX when coupled mode is engaged. | RW | 0x0 |
| 3:2 | Reserved. | - | - |
| 1 | EXPAND_EN: Enable the command expander. When 0, raw FIFO data is passed directly to the output shift register. When 1, the command expander can perform simple operations such as run length decoding on data between the FIFO and the shift register. Do not change CXPD_EN whilst EN is set. It’s safe to set CXPD_EN simultaneously with setting EN. | RW | 0x0 |
| 0 | EN: When EN is 1, the HSTX will shift out data as it appears in the FIFO. As long as there is data, the HSTX shift register will shift once per clock cycle, and the frequency of popping from the FIFO is determined by the ratio of SHIFT and SHIFT_THRESH. When EN is 0, the FIFO is not popped. The shift counter and clock generator are also reset to their initial state for as long as EN is low. Note the initial phase of the clock generator can be configured by the CLKPHASE field. Once the HSTX is enabled again, and data is pushed to the FIFO, the generated clock’s first rising edge will be one half-period after the first data is launched. | RW | 0x0 |

Table 1254. CSR

12.11. HSTX
1209

RP2350 Datasheet

HSTX_CTRL: BIT0, BIT1, …, BIT6, BIT7 Registers

Offsets: 0x04, 0x08, …, 0x1c, 0x20

Description

Data control register for output bit n

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:18 | Reserved. | - | - |
| 17 | CLK: Connect this output to the generated clock, rather than the data shift register. SEL_P and SEL_N are ignored if this bit is set, but INV can still be set to generate an antiphase clock. | RW | 0x0 |
| 16 | INV: Invert this data output (logical NOT) | RW | 0x0 |
| 15:13 | Reserved. | - | - |
| 12:8 | SEL_N: Shift register data bit select for the second half of the HSTX clock cycle | RW | 0x00 |
| 7:5 | Reserved. | - | - |
| 4:0 | SEL_P: Shift register data bit select for the first half of the HSTX clock cycle | RW | 0x00 |

Table 1255. BIT0,

BIT1, …, BIT6, BIT7

Registers

12.11. HSTX
1210

RP2350 Datasheet

HSTX_CTRL: EXPAND_SHIFT Register

Offset: 0x24

Description

Configure the optional shifter inside the command expander

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:29 | Reserved. | - | - |
| 28:24 | ENC_N_SHIFTS: Number of times to consume from the shift register before refilling it from the FIFO, when the current command is an encoded data command (e.g. TMDS). A register value of 0 means shift 32 times. | RW | 0x01 |
| 23:21 | Reserved. | - | - |
| 20:16 | ENC_SHIFT: How many bits to right-rotate the shift register by each time data is pushed to the output shifter, when the current command is an encoded data command (e.g. TMDS). | RW | 0x00 |
| 15:13 | Reserved. | - | - |
| 12:8 | RAW_N_SHIFTS: Number of times to consume from the shift register before refilling it from the FIFO, when the current command is a raw data command. A register value of 0 means shift 32 times. | RW | 0x01 |
| 7:5 | Reserved. | - | - |
| 4:0 | RAW_SHIFT: How many bits to right-rotate the shift register by each time data is pushed to the output shifter, when the current command is a raw data command. | RW | 0x00 |

Table 1256.

EXPAND_SHIFT

Register

HSTX_CTRL: EXPAND_TMDS Register

Offset: 0x28

Description

Configure the optional TMDS encoder inside the command expander

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:24 | Reserved. | - | - |
| 23:21 | L2_NBITS: Number of valid data bits for the lane 2 TMDS encoder, starting from bit 7 of the rotated data. Field values of 0 → 7 encode counts of 1 → 8 bits. | RW | 0x0 |
| 20:16 | L2_ROT: Right-rotate applied to the current shifter data before the lane 2 TMDS encoder. | RW | 0x00 |
| 15:13 | L1_NBITS: Number of valid data bits for the lane 1 TMDS encoder, starting from bit 7 of the rotated data. Field values of 0 → 7 encode counts of 1 → 8 bits. | RW | 0x0 |
| 12:8 | L1_ROT: Right-rotate applied to the current shifter data before the lane 1 TMDS encoder. | RW | 0x00 |
| 7:5 | L0_NBITS: Number of valid data bits for the lane 0 TMDS encoder, starting from bit 7 of the rotated data. Field values of 0 → 7 encode counts of 1 → 8 bits. | RW | 0x0 |
| 4:0 | L0_ROT: Right-rotate applied to the current shifter data before the lane 0 TMDS encoder. | RW | 0x00 |

Table 1257.

EXPAND_TMDS

Register

12.11. HSTX
1211

RP2350 Datasheet
