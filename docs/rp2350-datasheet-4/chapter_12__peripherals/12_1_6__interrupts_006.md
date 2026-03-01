---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.1.6. Interrupts
pages: 970-970
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.1.6. Interrupts

![Page 970 figure](images/fig_p0970.png)

12.1.6. Interrupts

There are eleven maskable interrupts generated in the UART. On RP2350, only the combined interrupt output, UARTINTR, is

connected.

To enable or disable individual interrupts, change the mask bits in the Interrupt Mask Set/Clear Register, UARTIMSC. Set

the appropriate mask bit HIGH to enable the interrupt.

The transmit and receive dataflow interrupts UARTRXINTR and UARTTXINTR have been separated from the status interrupts.

This enables you to use UARTRXINTR and UARTTXINTR to read or write data in response to FIFO trigger levels.

The error interrupt, UARTEINTR, can be triggered when there is an error in the reception of data. A number of error

conditions are possible.

The modem status interrupt, UARTMSINTR, is a combined interrupt of all the individual modem status signals.

The status of the individual interrupt sources can be read either from the Raw Interrupt Status Register, UARTRIS, or from

the Masked Interrupt Status Register, UARTMIS.

12.1.6.1. UARTMSINTR

The modem status interrupt is asserted if any of the modem status signals (nUARTCTS, nUARTDCD, nUARTDSR, and nUARTRI)

change. To clear the modem status interrupt, write a 1 to the bits corresponding to the modem status signals that

generated the interrupt in the Interrupt Clear Register (UARTICR).

12.1.6.2. UARTRXINTR

The receive interrupt changes state when one of the following events occurs:

• The FIFOs are enabled and the receive FIFO reaches the programmed trigger level. This asserts the receive

interrupt HIGH. To clear the receive interrupt, read data from the receive FIFO until it drops below the trigger level.
• The FIFOs are disabled (have a depth of one location) and data is received, thereby filling the receive FIFO. This

asserts the receive interrupt HIGH. To clear the receive interrupt, perform a single read from the receive FIFO.

In both cases, you can also clear the interrupt manually.

12.1.6.3. UARTTXINTR

The transmit interrupt changes state when one of the following events occurs:

• The FIFOs are enabled and the transmit FIFO is equal to or lower than the programmed trigger level. This asserts

the transmit interrupt HIGH. To clear the transmit interrupt, write data to the transmit FIFO until it exceeds the

12.1. UART
969
