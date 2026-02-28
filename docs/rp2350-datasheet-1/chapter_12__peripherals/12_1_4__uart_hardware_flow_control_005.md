---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.1.4. UART hardware flow control
pages: 967-967
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 12.1.4. UART hardware flow control

Table 1025. Receive
FIFO bit functions
FIFO bit
Function
11
Overrun indicator
10
Break error
9
Parity error
8
Framing error
7:0
Received data
12.1.3.2.5. Disabling the FIFOs
The bottom entry of the transmit and receive sides of the UART both have the equivalent of a 1-byte holding register.
You can manipulate flags to disable the FIFOs, allowing you to use the bottom entry of the FIFOs as a 1-byte register.
However, this doesn’t physically disable the FIFOs. When using the FIFOs as a 1-byte register, a write to the data register
bypasses the holding register unless the transmit shift register is already in use.
12.1.3.2.6. System and diagnostic loopback testing
To perform loopback testing for UART data, set the Loop Back Enable (LBE) bit to 1 in the Control Register, UARTCR.
Data transmitted on UARTTXD is received on the UARTRXD input.
12.1.3.3. UART character frame
Figure 65. UART
character frame.
12.1.4. UART hardware flow control
The fully-selectable hardware flow control feature enables you to control the serial data flow with the nUARTRTS output
and nUARTCTS input signals. Figure 66 shows how to communicate between two devices using hardware flow control:
Figure 66. Hardware
flow control between
two similar devices.
When the RTS flow control is enabled, nUARTRTS is asserted until the receive FIFO is filled up to the programmed
watermark level. When the CTS flow control is enabled, the transmitter can only transmit data when nUARTCTS is asserted.
The hardware flow control is selectable using the RTSEn and CTSEn bits in the Control Register, UARTCR. Table 1026 shows
how to configure UARTCR register bits to enable RTS and/or CTS.
RP2350 Datasheet
12.1. UART
966

