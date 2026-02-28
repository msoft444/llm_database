---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.1. Overview
pages: 877-877
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 11.1. Overview

Chapter 11. PIO
11.1. Overview
RP2350 contains 3 identical PIO blocks. Each PIO block has dedicated connections to the bus fabric, GPIO and interrupt
controller. The diagram for a single PIO block is shown below in Figure 44.
IRQ Masking
SM IRQs
Instruction Memory
32 Instructions
4 Read Ports
IRQ 0
IRQ 1
FIFO IRQs
FIFO
FIFO
FIFO
FIFO
FIFO
State Machine 3
State Machine 2
State Machine 1
State Machine 0
FIFO
FIFO
FIFO
Write only
IO Mapping
Figure 44. PIO block-
level diagram. There
are three PIO blocks,
each containing four
state machines. The
four state machines
simultaneously
execute programs
from shared
instruction memory.
FIFO data queues
buffer data transferred
between PIO and the
system. GPIO mapping
logic allows each
state machine to
observe and
manipulate up to 32
GPIOs.
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
RP2350 Datasheet
11.1. Overview
876

