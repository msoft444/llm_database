---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.1. Bus fabric
pages: 25-25
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 2.1. Bus fabric

![Page 25 figure](images/fig_p0025.png)

RP2350 Datasheet

Chapter 2. System bus

2.1. Bus fabric

The RP2350 bus fabric routes addresses and data across the chip.

Figure 5 shows the high-level structure of the bus fabric. The main AHB5 crossbar routes addresses and data between

its 6 upstream ports and 17 downstream ports, with up to six bus transfers taking place each cycle. All data paths are

32 bits wide. Memories connect to multiple dedicated ports on the main crossbar, for the best possible memory

bandwidth. High-bandwidth AHB peripherals share a port on the crossbar. An APB bridge provides access to system

control registers and lower-bandwidth peripherals. The SIO peripherals are accessed via a dedicated path from each

processor.

Figure 5. RP2350 bus

| Core 0
D |  | Core 1
I D |
| --- | --- | --- |
|  |  |  |
|  |  |  |
|  |  |  |

fabric overview.

DMA

R
W

Exclusive Query/ 

Response

Global Exclusivity 

AHB5 Crossbar

Monitor

SRAM Write Kill 

(SRAM0–9)

XIP Cache 

SRAM0–3 4 × 64 kB

SRAM8–9 

AHB5 

SRAM4–7 4 × 64 kB

to APB
ROM 32 kB

16 kB WBack 

Core 0
Port D

Word-striped

2× 4 kB

Word-striped

2-way 2-bank

Only

Core 1
Port D

Only

APB Splitter
Arbiter
AHB5 Splitter

PIO0 | PIO1

USB | DMA
Ctrl

Trace

FIFO
XIP 
Aux
Other 

SIO
UART0
UART1
I2C0
I2C1
SPI0
SPI1
PWM
PIO0
PIO1
PIO2
USB
Timer0
QSPI Memory 

APB

Interface

The bus fabric connects 6 AHB5 managers, i.e. bus ports which generate addresses:

• Core 0: Instruction port (instruction fetch), and Data port (load/store access)
• Core 1: Instruction port (instruction fetch), and Data port (load/store access)
• DMA controller: Read port, Write port

The following 13 downstream ports are symmetrically accessible from all 6 upstream ports:

• Boot ROM (1 port)
• XIP (2 ports, striped)
• SRAM (10 ports, striped)

Additionally, the following 2 ports are accessible for processor load/store and DMA read/write only:

• 1 shared port for fast AHB5 peripherals: PIO0, PIO1, PIO2, USB, DMA control registers, XIP DMA FIFOs, HSTX FIFO,

CoreSight trace DMA FIFO
• 1 port for the APB bridge, to all APB peripherals and control registers

2.1. Bus fabric
24
