---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.2. Interrupts
pages: 83-83
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 3.2. Interrupts

Table 92.
TMDS_POP_DOUBLE_L
1 Register
Bits
Description
Type
Reset
31:0
Get lane 1 of the encoding of two pixels' worth of colour data. Two 10-bit
TMDS symbols are packed at the bottom of a 32-bit word.
The POP alias shifts the colour register when read, according to the values of
PIX_SHIFT and PIX2_NOSHIFT.
RF
0x00000000
SIO: TMDS_PEEK_DOUBLE_L2 Register
Offset: 0x1e0
Table 93.
TMDS_PEEK_DOUBLE_
L2 Register
Bits
Description
Type
Reset
31:0
Get lane 2 of the encoding of two pixels' worth of colour data. Two 10-bit
TMDS symbols are packed at the bottom of a 32-bit word.
The PEEK alias does not shift the colour register when read, but still advances
the lane 2 DC balance state. This is useful if all 3 lanes' worth of encode are to
be read at once, rather than processing the entire scanline for one lane before
moving to the next lane.
RF
0x00000000
SIO: TMDS_POP_DOUBLE_L2 Register
Offset: 0x1e4
Table 94.
TMDS_POP_DOUBLE_L
2 Register
Bits
Description
Type
Reset
31:0
Get lane 2 of the encoding of two pixels' worth of colour data. Two 10-bit
TMDS symbols are packed at the bottom of a 32-bit word.
The POP alias shifts the colour register when read, according to the values of
PIX_SHIFT and PIX2_NOSHIFT.
RF
0x00000000
3.2. Interrupts
Each core is equipped with an internal interrupt controller, with 52 interrupt inputs. For the most part each core has
exactly the same interrupts routed to it, though there are some exceptions, referred to as core-local interrupts, where
there is an individual per-core interrupt source mapped to the same interrupt number on each core:
• Cross-core FIFO interrupts: SIO_IRQ_FIFO and SIO_IRQ_FIFO_NS (Section 3.1.5)
• Cross-core doorbell interrupts: SIO_IRQ_BELL and SIO_IRQ_BELL_NS (Section 3.1.6)
• RISC-V platform timer (also usable by Arm cores): SIO_IRQ_MTIMECMP (Section 3.1.8)
• GPIO interrupts: IO_IRQ_BANK0, IRQ_IO_BANK0_NS, IO_IRQ_QSPI, IO_IRQ_QSPI_NS (Section 9.5)
The remaining interrupt inputs have the same interrupt source mirrored identically on both cores. Non-core-local
interrupts should only be enabled in the interrupt controller of a single core at a time, and will be serviced by the core
whose interrupt controller they are enabled in.
Table 95. System-level
interrupt numbering.
All interrupts are
routed to both
processors.
IRQ
Interrupt Source
IRQ
Interrupt Source
IRQ
Interrupt Source
IRQ
Interrupt Source
IRQ
Interrupt Source
0
TIMER0_IRQ_0
11
DMA_IRQ_1
22
IO_IRQ_BANK0_NS
33
UART0_IRQ
44
POWMAN_IRQ_POW
1
TIMER0_IRQ_1
12
DMA_IRQ_2
23
IO_IRQ_QSPI
34
UART1_IRQ
45
POWMAN_IRQ_TIMER
2
TIMER0_IRQ_2
13
DMA_IRQ_3
24
IO_IRQ_QSPI_NS
35
ADC_IRQ_FIFO
46
SPAREIRQ_IRQ_0
RP2350 Datasheet
3.2. Interrupts
82

