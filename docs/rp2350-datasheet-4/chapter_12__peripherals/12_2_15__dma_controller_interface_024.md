---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.15. DMA controller interface
pages: 1008-1008
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.2.15. DMA controller interface

12.2.15. DMA controller interface

The DW_apb_i2c has built-in DMA capability; it has a handshaking interface to the DMA Controller to request and control

transfers. The APB bus is used to perform data transfers to and from the DMA. DMA transfers use single accesses,

since the data rate is relatively low.

12.2.15.1. Enabling the DMA controller interface

To enable the DMA Controller interface on the DW_apb_i2c, you must write the DMA Control Register (IC_DMA_CR).

Writing a one into the TDMAE bit field of IC_DMA_CR register enables the DW_apb_i2c transmit handshaking interface.

Writing a one into the RDMAE bit field of the IC_DMA_CR register enables the DW_apb_i2c receive handshaking interface.

12.2.15.2. Overview of operation

The DMA Controller is programmed with the number of data items (transfer count) that are to be transmitted or

received by DW_apb_i2c.

The transfer is broken into single transfers on the bus, each initiated by a request from the DW_apb_i2c.

12.2. I2C
1007
