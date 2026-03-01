---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.1.2. Functional description
pages: 963-964
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.1.2. Functional description

12.1.2. Functional description

![Page 963 figure](images/fig_p0963.png)

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
