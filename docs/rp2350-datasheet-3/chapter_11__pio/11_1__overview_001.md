---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.1. Overview
pages: 877-877
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 11.1. Overview

![Page 877 figure](images/fig_p0877.png)

RP2350 Datasheet

Chapter 11. PIO

11.1. Overview

RP2350 contains 3 identical PIO blocks. Each PIO block has dedicated connections to the bus fabric, GPIO and interrupt

controller. The diagram for a single PIO block is shown below in Figure 44.

Figure 44. PIO block-

IRQ 0

level diagram. There

IRQ Masking
SM IRQs

are three PIO blocks,

IRQ 1

each containing four

state machines. The

FIFO IRQs

four state machines

simultaneously

FIFO

execute programs

State Machine 0

from shared

FIFO

instruction memory.

FIFO data queues

FIFO

buffer data transferred

State Machine 1

between PIO and the

IO Mapping

FIFO

system. GPIO mapping

logic allows each

FIFO

state machine to

State Machine 2

observe and

FIFO

manipulate up to 32

GPIOs.

FIFO

State Machine 3

FIFO

Instruction Memory

Write only

32 Instructions

4 Read Ports

The programmable input/output block (PIO) is a versatile hardware interface. It can support a variety of IO standards,

including:

• 8080 and 6800 parallel bus
• I2C
• 3-pin I2S
• SDIO
• SPI, DSPI, QSPI
• UART
• DPI or VGA (via resistor DAC)

PIO is programmable in the same sense as a processor. There are three PIO blocks with four state machines. Each can

independently execute sequential programs to manipulate GPIOs and transfer data. Unlike a general purpose processor,

PIO state machines are specialised for IO, with a focus on determinism, precise timing, and close integration with fixed-

function hardware. Each state machine is equipped with:

• Two 32-bit shift registers (either direction, any shift count)
• Two 32-bit scratch registers
• 4 × 32-bit bus FIFO in each direction (TX/RX), reconfigurable as 8 × 32 in a single direction
• Fractional clock divider (16 integer, 8 fractional bits)

11.1. Overview
876
