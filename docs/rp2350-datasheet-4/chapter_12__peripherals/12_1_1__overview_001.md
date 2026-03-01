---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.1.1. Overview
pages: 962-962
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.1.1. Overview

12.1.1. Overview

The UART performs:

• Serial-to-parallel conversion on data received from a peripheral device
• Parallel-to-serial conversion on data transmitted to the peripheral device

The CPU reads and writes data and control/status information through the AMBA APB interface. The transmit and

receive paths are buffered with internal FIFO memories that store up to 32 bytes independently in both transmit and

receive modes.

The UART:

• Includes a programmable baud rate generator that generates a common transmit and receive internal clock from

the UART internal reference clock input, UARTCLK
• Offers similar functionality to the industry-standard 16C650 UART device
• Supports a maximum baud rate of UARTCLK / 16 in UART mode (7.8 Mbaud at 125MHz)

12.1. UART
961
