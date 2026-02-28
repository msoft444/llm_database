---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.1.1. Changes from RP2040
pages: 878-878
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 11.1.1. Changes from RP2040

RP2350 Datasheet

• Flexible GPIO mapping
• DMA interface (sustained throughput up to 1 word per clock from system DMA)
• IRQ flag set/clear/status

Each state machine, along with its supporting hardware, occupies approximately the same silicon area as a standard

serial interface block, such as an SPI or I2C controller. However, PIO state machines can be configured and

reconfigured dynamically to implement numerous different interfaces.

Making state machines programmable in a software-like manner, rather than a fully configurable logic fabric like a

complex programmable logic device (CPLD), allows more hardware interfaces to be offered in the same cost and power

envelope. This also presents a more familiar programming model, and simpler tool flow, to those who wish to exploit

PIO’s full flexibility by programming it directly, rather than using a pre-made interface from the PIO library.

PIO is performant as well as flexible, thanks to a carefully selected set of fixed-function hardware inside each state

machine. When outputting DPI, PIO can sustain 360 Mb/s during the active scanline period when running from a

48 MHz system clock. In this example, one state machine handles frame/scanline timing and generates the pixel clock.

Another handles the pixel data and unpacks run-length-encoded scanlines.

State machines' inputs and outputs are mapped to up to 32 GPIOs (limited to 30 GPIOs for RP2350). All state machines

have independent, simultaneous access to any GPIO. For example, the standard UART code allows TX, RX, CTS and RTS to

be any four arbitrary GPIOs, and I2C permits the same for SDA and SCL. The amount of freedom available depends on how

exactly a given PIO program chooses to use PIO’s pin mapping resources, but at the minimum, an interface can be

freely shifted up or down by some number of GPIOs.

11.1.1. Changes from RP2040

RP2350 adds the following new registers and controls:

• DBG_CFGINFO.VERSION indicates the PIO version, to allow PIO feature detection at runtime.

◦This 4-bit field was reserved-0 on RP2040 (indicating version 0), and reads as 1 on RP2350.
• GPIOBASE adds support for more than 32 GPIOs per PIO block.

◦Each PIO block is still limited to 32 GPIOs at a time, but GPIOBASE selects which 32.
• CTRL.NEXT_PIO_MASK and CTRL.PREV_PIO_MASK apply some CTRL register operations to state machines in

neighbouring PIO blocks simultaneously.

◦CTRL.NEXTPREV_SM_DISABLE stops PIO state machines in multiple PIO blocks simultaneously.

◦CTRL.NEXTPREV_SM_ENABLE starts PIO state machines in multiple PIO blocks simultaneously.

◦CTRL.NEXTPREV_CLKDIV_RESTART synchronises the clock dividers of PIO state machines in multiple PIO

blocks
• SM0_SHIFTCTRL.IN_COUNT masks unneeded IN-mapped pins to zero.

◦This is useful for MOV x, PINS instructions, which previously always returned a full rotated 32-bit value.
• IRQ0_INTE and IRQ1_INTE now expose all eight SM IRQ flags to system-level interrupts (not just the lower four).
• Registers starting from RXF0_PUTGET0 expose each RX FIFO’s internal storage registers for random read or write

access from the system,

◦The new FJOIN_RX_PUT FIFO join mode enables random writes from the state machine, and random reads from

the system (for implementing status registers).

◦The new FJOIN_RX_GET FIFO join mode enables random reads from the state machine, and random writes from

the system (for implementing control registers).

◦Setting both FJOIN_RX_PUT and FJOIN_RX_GET enables random read and write access from the state machine, but

disables system access.

11.1. Overview
877
