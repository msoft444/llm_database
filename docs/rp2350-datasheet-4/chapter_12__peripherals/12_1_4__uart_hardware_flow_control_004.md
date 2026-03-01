---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.1.4. UART hardware flow control
pages: 967-967
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.1.4. UART hardware flow control

![Page 967 figure](images/fig_p0967.png)

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

12.1. UART
966
