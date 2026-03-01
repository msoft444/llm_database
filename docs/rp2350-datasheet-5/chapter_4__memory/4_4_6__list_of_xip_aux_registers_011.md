---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 4. Memory
section: 4.4.6. List of XIP_AUX registers
pages: 351-353
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 4.4.6. List of XIP_AUX registers

![Page 351 figure](images/fig_p0351.png)

4.4.6. List of XIP_AUX registers

The XIP_AUX port provides fast AHB access to the streaming FIFO and the QMI Direct Mode FIFOs, to reduce the cost of

DMA access to these FIFOs.

| Offset | Name | Info |
| --- | --- | --- |
| 0x0 | STREAM | Read the XIP stream FIFO (fast bus access to XIP_CTRL_STREAM_FIFO) |
| 0x4 | QMI_DIRECT_TX | Write to the QMI direct-mode TX FIFO (fast bus access to QMI_DIRECT_TX) |
| 0x8 | QMI_DIRECT_RX | Read from the QMI direct-mode RX FIFO (fast bus access to QMI_DIRECT_RX) |

Table 447. List of

4.4. External flash and PSRAM (XIP)
350

RP2350 Datasheet

XIP_AUX: STREAM Register

Offset: 0x0

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Read the XIP stream FIFO (fast bus access to XIP_CTRL_STREAM_FIFO) | RF | 0x00000000 |
| 31:16 | Reserved. | - | - |
| 15:0 | With each byte clocked out on the serial interface, one byte will simultaneously be clocked in, and will appear in this FIFO. The serial interface will stall when this FIFO is full, to avoid dropping data. When 16-bit data is pushed into the TX FIFO, the corresponding RX FIFO push will also contain 16 bits of data. The least-significant byte is the first one received. | RF | 0x0000 |

Table 448. STREAM

XIP_AUX: QMI_DIRECT_TX Register

Offset: 0x4

Description

Write to the QMI direct-mode TX FIFO (fast bus access to QMI_DIRECT_TX)

![Page 352 figure](images/fig_p0352.png)

Table 449.

Bits
Description
Type
Reset

QMI_DIRECT_TX

Register

31:21
Reserved.
-
-

20
NOPUSH: Inhibit the RX FIFO push that would correspond to this TX FIFO

Useful to avoid garbage appearing in the RX FIFO when pushing the command

at the beginning of a SPI transfer.

19
OE: Output enable (active-high). For single width (SPI), this field is ignored, and

SD0 is always set to output, with SD1 always set to input.

For dual and quad width (DSPI/QSPI), this sets whether the relevant SDx pads

are set to output whilst transferring this FIFO record. In this case the

command/address should have OE set, and the data transfer should have OE

set or clear depending on the direction of the transfer.

18
DWIDTH: Data width. If 0, hardware will transmit the 8 LSBs of the DIRECT_TX

DATA field, and return an 8-bit value in the 8 LSBs of DIRECT_RX. If 1, the full

16-bit width is used. 8-bit and 16-bit transfers can be mixed freely.

17:16
IWIDTH: Configure whether this FIFO record is transferred with

single/dual/quad interface width (0/1/2). Different widths can be mixed freely.

15:0
DATA: Data pushed here will be clocked out falling edges of SCK (or before

the very first rising edge of SCK, if this is the first pulse). For each byte clocked

out, the interface will simultaneously sample one byte, on rising edges of SCK,

and push this to the DIRECT_RX FIFO.

For 16-bit data, the least-significant byte is transmitted first.

XIP_AUX: QMI_DIRECT_RX Register

Offset: 0x8

4.4. External flash and PSRAM (XIP)
351

RP2350 Datasheet

Description

Read from the QMI direct-mode RX FIFO (fast bus access to QMI_DIRECT_RX)

Table 450.

QMI_DIRECT_RX

Register
