---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.1.2. Functional description
pages: 963-964
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.1.2. Functional description

![Page 963 figure](images/fig_p0963.png)

RP2350 Datasheet

The UART operation and baud rate values are controlled by the Line Control Register (UARTLCR_H) and the baud rate

divisor registers: Integer Baud Rate Register (UARTIBRD), and Fractional Baud Rate Register (UARTFBRD).

The UART can generate:

• Individually maskable interrupts from the receive (including timeout), transmit, modem status and error conditions
• A single combined interrupt so that the output is asserted if any of the individual interrupts are asserted and

unmasked
• DMA request signals for interfacing with a Direct Memory Access (DMA) controller

If a framing, parity, or break error occurs during reception, the appropriate error bit is set and stored in the FIFO. If an

overrun condition occurs, the overrun register bit is set immediately and FIFO data is prevented from being overwritten.

You can program the FIFOs to be 1-byte deep providing a conventional double-buffered UART interface.

There is a programmable hardware flow control feature that uses the nUARTCTS input and the nUARTRTS output to

automatically control the serial data flow.

12.1.2. Functional description

Figure 63. UART block

diagram. Test logic is

not shown for clarity.

12.1.2.1. AMBA APB interface

The AMBA APB interface generates read and write decodes for accesses to status/control registers, and the transmit

and receive FIFOs.

12.1.2.2. Register block

The register block stores data written, or to be read across the AMBA APB interface.

12.1. UART
962

RP2350 Datasheet

12.1.2.3. Baud rate generator

The baud rate generator contains free-running counters that generate the internal clocks: Baud16 and IrLPBaud16

signals. Baud16 provides timing information for UART transmit and receive control. Baud16 is a stream of pulses with a

width of one UARTCLK clock period and a frequency of 16 times the baud rate.

12.1.2.4. Transmit FIFO

The transmit FIFO is an 8-bit wide, 32 location deep, FIFO memory buffer. CPU data written across the APB interface is

stored in the FIFO until read out by the transmit logic. When disabled, the transmit FIFO acts like a one byte holding

register.

12.1.2.5. Receive FIFO

The receive FIFO is a 12-bit wide, 32 location deep, FIFO memory buffer. Received data and corresponding error bits are

stored in the receive FIFO by the receive logic until read out by the CPU across the APB interface. When disabled, the

receive FIFO acts like a one byte holding register.

12.1.2.6. Transmit logic

The transmit logic performs parallel-to-serial conversion on the data read from the transmit FIFO. Control logic outputs

the serial bit stream in the following order:

1. Start bit

2. Data bits (Least Significant Bit (LSB) first)

3. Parity bit

4. Stop bits according to the programmed configuration in control registers

12.1.2.7. Receive logic

The receive logic performs serial-to-parallel conversion on the received bit stream after a valid start pulse has been

detected. Receive logic includes overrun, parity, frame error checking, and line break detection; you can find the output

of these checks in the status that accompanies the data written to the receive FIFO.

12.1.2.8. Interrupt generation logic

The UART generates individual maskable active HIGH interrupts to the processor interrupt controllers. To generate

combined interrupts, the UART outputs an OR function of the individual interrupt requests.

For more information, see Section 12.1.6.

12.1.2.9. DMA interface

The UART provides an interface to connect to the DMA controller as a UART DMA; for more information, see Section

12.1.5.

12.1. UART
963
